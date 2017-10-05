---
title: "Kurz: Azure Active Directory integrace s pokyny, společnost Microsoft | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a pokynů společnost Microsoft."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e0c8986f-2acd-418d-a306-437abc44b640
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: f9c068c71eb00a4c779c91c8ee0f0dc9d6ba85ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a><span data-ttu-id="49c98-103">Kurz: Azure Active Directory integrace s pokyny, společnost Microsoft</span><span class="sxs-lookup"><span data-stu-id="49c98-103">Tutorial: Azure Active Directory integration with Directions on Microsoft</span></span>

<span data-ttu-id="49c98-104">V tomto kurzu zjistěte, jak mají být integrovány pokynů na Microsoft Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="49c98-104">In this tutorial, you learn how to integrate Directions on Microsoft with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="49c98-105">Integrace s Azure AD pokynů společnost Microsoft poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="49c98-105">Integrating Directions on Microsoft with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="49c98-106">Můžete řídit ve službě Azure AD, který má přístup k pokynů na Microsoft</span><span class="sxs-lookup"><span data-stu-id="49c98-106">You can control in Azure AD who has access to Directions on Microsoft</span></span>
- <span data-ttu-id="49c98-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k pokynů společnost Microsoft (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="49c98-107">You can enable your users to automatically get signed-on to Directions on Microsoft (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="49c98-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="49c98-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="49c98-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="49c98-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49c98-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="49c98-110">Prerequisites</span></span>

<span data-ttu-id="49c98-111">Konfigurace integrace Azure AD s pokyny, společnost Microsoft, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="49c98-111">To configure Azure AD integration with Directions on Microsoft, you need the following items:</span></span>

- <span data-ttu-id="49c98-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="49c98-112">An Azure AD subscription</span></span>
- <span data-ttu-id="49c98-113">Předplatné povolené pokynů na Microsoft Jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="49c98-113">A Directions on Microsoft single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="49c98-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="49c98-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="49c98-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="49c98-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="49c98-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="49c98-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="49c98-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="49c98-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="49c98-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="49c98-118">Scenario description</span></span>
<span data-ttu-id="49c98-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="49c98-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="49c98-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="49c98-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="49c98-121">Přidání pokynů na Microsoft z Galerie</span><span class="sxs-lookup"><span data-stu-id="49c98-121">Adding Directions on Microsoft from the gallery</span></span>
2. <span data-ttu-id="49c98-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="49c98-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-directions-on-microsoft-from-the-gallery"></a><span data-ttu-id="49c98-123">Přidání pokynů na Microsoft z Galerie</span><span class="sxs-lookup"><span data-stu-id="49c98-123">Adding Directions on Microsoft from the gallery</span></span>
<span data-ttu-id="49c98-124">Konfigurace integrace pokynů na Microsoft do služby Azure AD, potřebujete přidat pokynů na Microsoft z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="49c98-124">To configure the integration of Directions on Microsoft into Azure AD, you need to add Directions on Microsoft from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="49c98-125">**Přidat pokynů na Microsoft z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="49c98-125">**To add Directions on Microsoft from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="49c98-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="49c98-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="49c98-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="49c98-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="49c98-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="49c98-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="49c98-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="49c98-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="49c98-133">Do vyhledávacího pole zadejte **pokynů na Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="49c98-133">In the search box, type **Directions on Microsoft**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_search.png)

5. <span data-ttu-id="49c98-135">Na panelu výsledků vyberte **pokynů na Microsoft**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="49c98-135">In the results panel, select **Directions on Microsoft**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="49c98-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="49c98-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="49c98-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování pokyny týkající se společnosti Microsoft podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="49c98-138">In this section, you configure and test Azure AD single sign-on with Directions on Microsoft based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="49c98-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějšek v směrech na Microsoft je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="49c98-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Directions on Microsoft is to a user in Azure AD.</span></span> <span data-ttu-id="49c98-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v směrech na Microsoft musí navázat.</span><span class="sxs-lookup"><span data-stu-id="49c98-140">In other words, a link relationship between an Azure AD user and the related user in Directions on Microsoft needs to be established.</span></span>

<span data-ttu-id="49c98-141">V pokynů na Microsoft přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="49c98-141">In Directions on Microsoft, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="49c98-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s pokyny, společnost Microsoft, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="49c98-142">To configure and test Azure AD single sign-on with Directions on Microsoft, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="49c98-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="49c98-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="49c98-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="49c98-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="49c98-145">**[Vytváření pokynů na Microsoft testovacího uživatele](#creating-a-directions-on-microsoft-test-user)**  – Pokud chcete mít protějšek Britta Simon v směrech na společnosti Microsoft, který je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="49c98-145">**[Creating a Directions on Microsoft test user](#creating-a-directions-on-microsoft-test-user)** - to have a counterpart of Britta Simon in Directions on Microsoft that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="49c98-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="49c98-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="49c98-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="49c98-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="49c98-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="49c98-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="49c98-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v vaší směrech na aplikaci Microsoft.</span><span class="sxs-lookup"><span data-stu-id="49c98-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Directions on Microsoft application.</span></span>

<span data-ttu-id="49c98-150">**Chcete-li nakonfigurovat pokynů na Microsoft Azure AD jednotné přihlašování, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="49c98-150">**To configure Azure AD single sign-on with Directions on Microsoft, perform the following steps:**</span></span>

1. <span data-ttu-id="49c98-151">Na portálu Azure na **pokynů na Microsoft** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="49c98-151">In the Azure portal, on the **Directions on Microsoft** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="49c98-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="49c98-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_samlbase.png)

3. <span data-ttu-id="49c98-155">Na **pokynů na Microsoft Domain a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="49c98-155">On the **Directions on Microsoft Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_url.png)

    <span data-ttu-id="49c98-157">a.</span><span class="sxs-lookup"><span data-stu-id="49c98-157">a.</span></span> <span data-ttu-id="49c98-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="49c98-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>
    |  |
    | --- |
    | `https://www.directionsonmicrosoft.com/user/login` |
    | `https://<subdomain>.devcloud.acquia-sites.com/<companyname>` |

    <span data-ttu-id="49c98-159">b.</span><span class="sxs-lookup"><span data-stu-id="49c98-159">b.</span></span> <span data-ttu-id="49c98-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="49c98-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    |  |
    | --- |
    | `https://rhelmdirectionsonmicrosoftcomtest.devcloud.acquia-sites.com/simplesaml/<companyname>` |
    | `https://www.directionsonmicrosoft.com/simplesaml/<companyname>` |

    > [!NOTE] 
    > <span data-ttu-id="49c98-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="49c98-161">These values are not real.</span></span> <span data-ttu-id="49c98-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="49c98-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="49c98-163">Obraťte se na [tým podpory pokynů na Microsoft Client](mailto:service@DirectionsOnMicrosoft.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="49c98-163">Contact [Directions on Microsoft Client support team](mailto:service@DirectionsOnMicrosoft.com) to get these values.</span></span> 
 
4. <span data-ttu-id="49c98-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="49c98-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_certificate.png) 

5. <span data-ttu-id="49c98-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="49c98-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="49c98-168">Konfigurace jednotného přihlašování na **pokynů na Microsoft** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory pokynů na Microsoft](mailto:service@DirectionsOnMicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="49c98-168">To configure single sign-on on **Directions on Microsoft** side, you need to send the downloaded **Metadata XML** to [Directions on Microsoft support team](mailto:service@DirectionsOnMicrosoft.com).</span></span> <span data-ttu-id="49c98-169">Pokud chcete povolit pokynů na tým podpory společnosti Microsoft vyhledejte členstvím na vašem federovaného webu, zahrnují informace o vaší společnosti v e-mailu.</span><span class="sxs-lookup"><span data-stu-id="49c98-169">To enable the Directions on Microsoft support team to locate your federated site membership, include your company information in your email.</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="49c98-170">Jednotné přihlašování pro pokynů na Microsoft musí být povolena ve [tým podpory pokynů na Microsoft Client](mailto:service@DirectionsOnMicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="49c98-170">Single sign-on for Directions on Microsoft needs to be enabled by the [Directions on Microsoft Client support team](mailto:service@DirectionsOnMicrosoft.com).</span></span> <span data-ttu-id="49c98-171">Zobrazí se oznámení a pokud jednotné přihlašování je povolena.</span><span class="sxs-lookup"><span data-stu-id="49c98-171">You will receive a notification when single sign-on has been enabled.</span></span>

> [!TIP]
> <span data-ttu-id="49c98-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="49c98-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="49c98-173">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="49c98-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="49c98-174">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="49c98-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="49c98-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="49c98-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="49c98-176">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="49c98-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="49c98-178">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="49c98-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="49c98-179">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="49c98-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="49c98-181">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="49c98-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="49c98-183">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="49c98-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="49c98-185">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="49c98-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="49c98-187">a.</span><span class="sxs-lookup"><span data-stu-id="49c98-187">a.</span></span> <span data-ttu-id="49c98-188">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="49c98-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="49c98-189">b.</span><span class="sxs-lookup"><span data-stu-id="49c98-189">b.</span></span> <span data-ttu-id="49c98-190">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="49c98-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="49c98-191">c.</span><span class="sxs-lookup"><span data-stu-id="49c98-191">c.</span></span> <span data-ttu-id="49c98-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="49c98-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="49c98-193">d.</span><span class="sxs-lookup"><span data-stu-id="49c98-193">d.</span></span> <span data-ttu-id="49c98-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="49c98-194">Click **Create**.</span></span>
 
### <a name="creating-a-directions-on-microsoft-test-user"></a><span data-ttu-id="49c98-195">Vytváření pokynů na Microsoft testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="49c98-195">Creating a Directions on Microsoft test user</span></span>

<span data-ttu-id="49c98-196">Neexistuje žádná položka akce můžete nakonfigurovat uživatele zajišťování, které pokynů společnost Microsoft.</span><span class="sxs-lookup"><span data-stu-id="49c98-196">There is no action item for you to configure user provisioning to Directions on Microsoft.</span></span>  

<span data-ttu-id="49c98-197">Když přiřazený uživatel se pokusí přihlásit k pokynů na Microsoft použije na přístupovém panelu, pokynů na Microsoft ověří, zda uživatel existuje.</span><span class="sxs-lookup"><span data-stu-id="49c98-197">When an assigned user tries to log in to Directions on Microsoft using the access panel, Directions on Microsoft checks whether the user exists.</span></span> <span data-ttu-id="49c98-198">Pokud neexistuje žádný účet k dispozici dosud, se automaticky vytvoří podle pokynů na Microsoft.</span><span class="sxs-lookup"><span data-stu-id="49c98-198">If there is no user account available yet, it is automatically created by Directions on Microsoft.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="49c98-199">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="49c98-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="49c98-200">V této části povolíte Britta Simon používat Azure jednotné přihlašování podle pokynů na Microsoft udělení přístupu.</span><span class="sxs-lookup"><span data-stu-id="49c98-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Directions on Microsoft.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="49c98-202">**Pokud chcete přiřadit Britta Simon pokynů společnost Microsoft, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="49c98-202">**To assign Britta Simon to Directions on Microsoft, perform the following steps:**</span></span>

1. <span data-ttu-id="49c98-203">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="49c98-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="49c98-205">V seznamu aplikací vyberte **pokynů na Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="49c98-205">In the applications list, select **Directions on Microsoft**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_app.png) 

3. <span data-ttu-id="49c98-207">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="49c98-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="49c98-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="49c98-209">Click **Add** button.</span></span> <span data-ttu-id="49c98-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="49c98-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="49c98-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="49c98-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="49c98-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="49c98-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="49c98-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="49c98-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="49c98-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="49c98-215">Testing single sign-on</span></span>

<span data-ttu-id="49c98-216">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="49c98-216">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>
 
<span data-ttu-id="49c98-217">Když kliknete na dlaždici Microsoft na přístupovém panelu pokynů, jste měli získat automaticky přihlášení k vaší pokynů na aplikaci Microsoft.</span><span class="sxs-lookup"><span data-stu-id="49c98-217">When you click the Directions on Microsoft tile in the Access Panel, you should get automatically signed-on to your Directions on Microsoft application.</span></span>

<span data-ttu-id="49c98-218">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="49c98-218">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="49c98-219">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="49c98-219">Additional resources</span></span>

* [<span data-ttu-id="49c98-220">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="49c98-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="49c98-221">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="49c98-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_203.png

