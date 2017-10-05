---
title: 'Kurz: Azure Active Directory integrace s ThirdLight | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ThirdLight."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 168aae9a-54ee-4c2b-ab12-650a2c62b901
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: ee7710cfea3a13907c0cc940a98c875bf83607a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-thirdlight"></a><span data-ttu-id="f3273-103">Kurz: Azure Active Directory integrace s ThirdLight</span><span class="sxs-lookup"><span data-stu-id="f3273-103">Tutorial: Azure Active Directory integration with ThirdLight</span></span>

<span data-ttu-id="f3273-104">V tomto kurzu zjistěte, jak integrovat ThirdLight s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f3273-104">In this tutorial, you learn how to integrate ThirdLight with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f3273-105">Integrace ThirdLight s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f3273-105">Integrating ThirdLight with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f3273-106">Můžete řídit ve službě Azure AD, který má přístup k ThirdLight</span><span class="sxs-lookup"><span data-stu-id="f3273-106">You can control in Azure AD who has access to ThirdLight</span></span>
- <span data-ttu-id="f3273-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k ThirdLight (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="f3273-107">You can enable your users to automatically get signed-on to ThirdLight (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f3273-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f3273-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="f3273-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f3273-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f3273-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f3273-110">Prerequisites</span></span>

<span data-ttu-id="f3273-111">Konfigurace integrace Azure AD s ThirdLight, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="f3273-111">To configure Azure AD integration with ThirdLight, you need the following items:</span></span>

- <span data-ttu-id="f3273-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f3273-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f3273-113">ThirdLight jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="f3273-113">A ThirdLight single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f3273-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f3273-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f3273-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="f3273-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f3273-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="f3273-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f3273-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f3273-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f3273-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f3273-118">Scenario description</span></span>
<span data-ttu-id="f3273-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f3273-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f3273-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="f3273-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f3273-121">Přidání ThirdLight z Galerie</span><span class="sxs-lookup"><span data-stu-id="f3273-121">Adding ThirdLight from the gallery</span></span>
2. <span data-ttu-id="f3273-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f3273-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-thirdlight-from-the-gallery"></a><span data-ttu-id="f3273-123">Přidání ThirdLight z Galerie</span><span class="sxs-lookup"><span data-stu-id="f3273-123">Adding ThirdLight from the gallery</span></span>
<span data-ttu-id="f3273-124">Při konfiguraci integrace ThirdLight do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS ThirdLight z galerie.</span><span class="sxs-lookup"><span data-stu-id="f3273-124">To configure the integration of ThirdLight into Azure AD, you need to add ThirdLight from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f3273-125">**Pokud chcete přidat ThirdLight z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f3273-125">**To add ThirdLight from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f3273-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f3273-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f3273-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f3273-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f3273-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f3273-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="f3273-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f3273-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="f3273-133">Do vyhledávacího pole zadejte **ThirdLight**.</span><span class="sxs-lookup"><span data-stu-id="f3273-133">In the search box, type **ThirdLight**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_search.png)

5. <span data-ttu-id="f3273-135">Na panelu výsledků vyberte **ThirdLight**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f3273-135">In the results panel, select **ThirdLight**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f3273-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f3273-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f3273-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s ThirdLight podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="f3273-138">In this section, you configure and test Azure AD single sign-on with ThirdLight based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f3273-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v ThirdLight je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f3273-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ThirdLight is to a user in Azure AD.</span></span> <span data-ttu-id="f3273-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v ThirdLight musí navázat.</span><span class="sxs-lookup"><span data-stu-id="f3273-140">In other words, a link relationship between an Azure AD user and the related user in ThirdLight needs to be established.</span></span>

<span data-ttu-id="f3273-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v ThirdLight.</span><span class="sxs-lookup"><span data-stu-id="f3273-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in ThirdLight.</span></span>

<span data-ttu-id="f3273-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s ThirdLight, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="f3273-142">To configure and test Azure AD single sign-on with ThirdLight, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f3273-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="f3273-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f3273-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f3273-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f3273-145">**[Vytvoření zkušebního uživatele ThirdLight](#creating-a-thirdlight-test-user)**  – Pokud chcete mít protějšek Britta Simon v ThirdLight propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="f3273-145">**[Creating a ThirdLight test user](#creating-a-thirdlight-test-user)** - to have a counterpart of Britta Simon in ThirdLight that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f3273-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f3273-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f3273-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f3273-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f3273-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f3273-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f3273-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci ThirdLight.</span><span class="sxs-lookup"><span data-stu-id="f3273-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ThirdLight application.</span></span>

<span data-ttu-id="f3273-150">**Ke konfiguraci Azure AD jednotné přihlašování s ThirdLight, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f3273-150">**To configure Azure AD single sign-on with ThirdLight, perform the following steps:**</span></span>

1. <span data-ttu-id="f3273-151">Na portálu Azure na **ThirdLight** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f3273-151">In the Azure portal, on the **ThirdLight** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="f3273-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f3273-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_samlbase.png)

3. <span data-ttu-id="f3273-155">Na **ThirdLight domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f3273-155">On the **ThirdLight Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_url.png)

    <span data-ttu-id="f3273-157">a.</span><span class="sxs-lookup"><span data-stu-id="f3273-157">a.</span></span> <span data-ttu-id="f3273-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.thirdlight.com/`</span><span class="sxs-lookup"><span data-stu-id="f3273-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.thirdlight.com/`</span></span>

    <span data-ttu-id="f3273-159">b.</span><span class="sxs-lookup"><span data-stu-id="f3273-159">b.</span></span> <span data-ttu-id="f3273-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.thirdlight.com/saml/sp`</span><span class="sxs-lookup"><span data-stu-id="f3273-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.thirdlight.com/saml/sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f3273-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="f3273-161">These values are not real.</span></span> <span data-ttu-id="f3273-162">Tyto hodnoty aktualizujte skutečná adresa URL přihlašování a Identiifer.</span><span class="sxs-lookup"><span data-stu-id="f3273-162">Update these values with the actual Sign-On URL and Identiifer.</span></span> <span data-ttu-id="f3273-163">Obraťte se na [tým podpory ThirdLight klienta](https://www.thirdlight.com/support) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="f3273-163">Contact [ThirdLight Client support team](https://www.thirdlight.com/support) to get these values.</span></span> 
 
4. <span data-ttu-id="f3273-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="f3273-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_certificate.png) 

5. <span data-ttu-id="f3273-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f3273-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thirdlight-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f3273-168">V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti ThirdLight jako správce.</span><span class="sxs-lookup"><span data-stu-id="f3273-168">In a different web browser window, log in to your ThirdLight company site as an administrator.</span></span>

7. <span data-ttu-id="f3273-169">Přejděte na **konfigurace \> systému správy**a potom klikněte na **typu SAML2**.</span><span class="sxs-lookup"><span data-stu-id="f3273-169">Go to **Configuration \> System Administration**, and then click **SAML2**.</span></span>
   
    <span data-ttu-id="f3273-170">![Správa systému](./media/active-directory-saas-thirdlight-tutorial/ic805843.png "správu systému")</span><span class="sxs-lookup"><span data-stu-id="f3273-170">![System Administration](./media/active-directory-saas-thirdlight-tutorial/ic805843.png "System Administration")</span></span>

8. <span data-ttu-id="f3273-171">V konfiguračním oddílu typu SAML2 proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f3273-171">In the SAML2 configuration section, perform the following steps:</span></span>
   
    <span data-ttu-id="f3273-172">![SAML Single Sign-On](./media/active-directory-saas-thirdlight-tutorial/ic805844.png "SAML jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="f3273-172">![SAML Single Sign-On](./media/active-directory-saas-thirdlight-tutorial/ic805844.png "SAML Single Sign-On")</span></span>   

     <span data-ttu-id="f3273-173">a.</span><span class="sxs-lookup"><span data-stu-id="f3273-173">a.</span></span> <span data-ttu-id="f3273-174">Vyberte **povolit typu SAML2 Single Sign-On**.</span><span class="sxs-lookup"><span data-stu-id="f3273-174">Select **Enable SAML2 Single Sign-On**.</span></span>
 
     <span data-ttu-id="f3273-175">b.</span><span class="sxs-lookup"><span data-stu-id="f3273-175">b.</span></span> <span data-ttu-id="f3273-176">Jako **zdroj pro IdP Metadata**, vyberte **zatížení IdP metadat ze souboru XML**.</span><span class="sxs-lookup"><span data-stu-id="f3273-176">As **Source for IdP Metadata**, select **Load IdP Metadata from XML**.</span></span>
 
     <span data-ttu-id="f3273-177">c.</span><span class="sxs-lookup"><span data-stu-id="f3273-177">c.</span></span> <span data-ttu-id="f3273-178">Otevřete soubor stažený metadat, kopírovat obsah a vložte ji do **soubor XML s metadaty IdP** textové pole.</span><span class="sxs-lookup"><span data-stu-id="f3273-178">Open the downloaded metadata file, copy the content, and then paste it into the **IdP Metadata XML** textbox.</span></span> 
     
     <span data-ttu-id="f3273-179">d.</span><span class="sxs-lookup"><span data-stu-id="f3273-179">d.</span></span> <span data-ttu-id="f3273-180">Klikněte na tlačítko **nastavení uložit typu SAML2**.</span><span class="sxs-lookup"><span data-stu-id="f3273-180">Click **Save SAML2 settings**.</span></span>

> [!TIP]
> <span data-ttu-id="f3273-181">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="f3273-181">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f3273-182">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="f3273-182">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f3273-183">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f3273-183">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f3273-184">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f3273-184">Creating an Azure AD test user</span></span>
<span data-ttu-id="f3273-185">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f3273-185">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="f3273-187">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f3273-187">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f3273-188">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f3273-188">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f3273-190">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="f3273-190">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f3273-192">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f3273-192">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f3273-194">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f3273-194">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thirdlight-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f3273-196">a.</span><span class="sxs-lookup"><span data-stu-id="f3273-196">a.</span></span> <span data-ttu-id="f3273-197">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f3273-197">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f3273-198">b.</span><span class="sxs-lookup"><span data-stu-id="f3273-198">b.</span></span> <span data-ttu-id="f3273-199">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f3273-199">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="f3273-200">c.</span><span class="sxs-lookup"><span data-stu-id="f3273-200">c.</span></span> <span data-ttu-id="f3273-201">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="f3273-201">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f3273-202">d.</span><span class="sxs-lookup"><span data-stu-id="f3273-202">d.</span></span> <span data-ttu-id="f3273-203">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f3273-203">Click **Create**.</span></span>
 
### <a name="creating-a-thirdlight-test-user"></a><span data-ttu-id="f3273-204">Vytvoření zkušebního uživatele ThirdLight</span><span class="sxs-lookup"><span data-stu-id="f3273-204">Creating a ThirdLight test user</span></span>

<span data-ttu-id="f3273-205">Pokud chcete povolit uživatelům Azure AD přihlášení k ThirdLight, musí být zřízená do ThirdLight.</span><span class="sxs-lookup"><span data-stu-id="f3273-205">To enable Azure AD users to log in to ThirdLight, they must be provisioned into ThirdLight.</span></span>  
<span data-ttu-id="f3273-206">V případě ThirdLight zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="f3273-206">In the case of ThirdLight, provisioning is a manual task.</span></span>

<span data-ttu-id="f3273-207">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f3273-207">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="f3273-208">Přihlaste se k vaší **ThirdLight** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="f3273-208">Log in to your **ThirdLight** company site as an administrator.</span></span>

2. <span data-ttu-id="f3273-209">Přejděte na **uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="f3273-209">Go to **Users** tab.</span></span>

3. <span data-ttu-id="f3273-210">Vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f3273-210">Select **Users and Groups**.</span></span>

4. <span data-ttu-id="f3273-211">Klikněte na tlačítko **přidat nového uživatele** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f3273-211">Click **Add new User** button.</span></span>

5. <span data-ttu-id="f3273-212">Zadejte **uživatelské jméno, název nebo popis, e-mailu, vyberte přednastavení nebo skupiny nové členy** platného účtu AAD chcete zřídit.</span><span class="sxs-lookup"><span data-stu-id="f3273-212">Enter **the Username, Name or Description, Email, Choose a Preset or Group of New Members** of a valid AAD account you want to provision.</span></span>

6. <span data-ttu-id="f3273-213">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f3273-213">Click **Create**.</span></span>

>[!NOTE]
><span data-ttu-id="f3273-214">Můžete použít všechny ostatní Thirdlight uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Thirdlight zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="f3273-214">You can use any other Thirdlight user account creation tools or APIs provided by Thirdlight to provision AAD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f3273-215">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f3273-215">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f3273-216">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu ThirdLight.</span><span class="sxs-lookup"><span data-stu-id="f3273-216">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ThirdLight.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="f3273-218">**Pokud chcete přiřadit Britta Simon ThirdLight, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f3273-218">**To assign Britta Simon to ThirdLight, perform the following steps:**</span></span>

1. <span data-ttu-id="f3273-219">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f3273-219">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="f3273-221">V seznamu aplikací vyberte **ThirdLight**.</span><span class="sxs-lookup"><span data-stu-id="f3273-221">In the applications list, select **ThirdLight**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thirdlight-tutorial/tutorial_thirdlight_app.png) 

3. <span data-ttu-id="f3273-223">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f3273-223">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="f3273-225">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f3273-225">Click **Add** button.</span></span> <span data-ttu-id="f3273-226">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f3273-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="f3273-228">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="f3273-228">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f3273-229">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f3273-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f3273-230">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f3273-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f3273-231">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f3273-231">Testing single sign-on</span></span>

<span data-ttu-id="f3273-232">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="f3273-232">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f3273-233">Když kliknete na dlaždici ThirdLight na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci ThirdLight.</span><span class="sxs-lookup"><span data-stu-id="f3273-233">When you click the ThirdLight tile in the Access Panel, you should get automatically signed-on to your ThirdLight application.</span></span>
<span data-ttu-id="f3273-234">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f3273-234">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f3273-235">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f3273-235">Additional resources</span></span>

* [<span data-ttu-id="f3273-236">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f3273-236">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f3273-237">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f3273-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thirdlight-tutorial/tutorial_general_203.png

