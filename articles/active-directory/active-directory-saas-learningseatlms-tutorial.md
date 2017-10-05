---
title: "Kurz: Azure Active Directory integrace s Learning stanici pro správu vzdělávacího procesu | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Learning stanici pro správu vzdělávacího procesu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bb056fcf-4135-478e-85b1-5015d1f07b85
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: jeedes
ms.openlocfilehash: 877e0288fdd1f590acf064c204aff0741539b112
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learning-seat-lms"></a><span data-ttu-id="3ec37-103">Kurz: Azure Active Directory integrace s Learning stanici pro správu vzdělávacího procesu</span><span class="sxs-lookup"><span data-stu-id="3ec37-103">Tutorial: Azure Active Directory integration with Learning Seat LMS</span></span>

<span data-ttu-id="3ec37-104">V tomto kurzu zjistěte, jak integrovat vzdělávacího procesu stanici Learning s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3ec37-104">In this tutorial, you learn how to integrate Learning Seat LMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3ec37-105">Integrace vzdělávacího procesu stanici Learning s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3ec37-105">Integrating Learning Seat LMS with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3ec37-106">Můžete řídit ve službě Azure AD, který má přístup ke stanici Learning vzdělávacího procesu</span><span class="sxs-lookup"><span data-stu-id="3ec37-106">You can control in Azure AD who has access to Learning Seat LMS</span></span>
- <span data-ttu-id="3ec37-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Learning stanici správu vzdělávacího procesu (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="3ec37-107">You can enable your users to automatically get signed-on to Learning Seat LMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3ec37-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3ec37-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3ec37-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, přečtěte si téma.</span><span class="sxs-lookup"><span data-stu-id="3ec37-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="3ec37-110">[Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3ec37-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3ec37-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3ec37-111">Prerequisites</span></span>

<span data-ttu-id="3ec37-112">Konfigurace integrace Azure AD s Learning stanici pro správu vzdělávacího procesu, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="3ec37-112">To configure Azure AD integration with Learning Seat LMS, you need the following items:</span></span>

- <span data-ttu-id="3ec37-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3ec37-113">An Azure AD subscription</span></span>
- <span data-ttu-id="3ec37-114">Stanici Learning vzdělávacího procesu jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="3ec37-114">A Learning Seat LMS single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3ec37-115">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3ec37-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3ec37-116">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3ec37-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3ec37-117">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3ec37-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3ec37-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3ec37-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3ec37-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3ec37-119">Scenario description</span></span>
<span data-ttu-id="3ec37-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3ec37-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3ec37-121">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3ec37-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3ec37-122">Přidání Learning stanici pro správu vzdělávacího procesu z Galerie</span><span class="sxs-lookup"><span data-stu-id="3ec37-122">Adding Learning Seat LMS from the gallery</span></span>
2. <span data-ttu-id="3ec37-123">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3ec37-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learning-seat-lms-from-the-gallery"></a><span data-ttu-id="3ec37-124">Přidání Learning stanici pro správu vzdělávacího procesu z Galerie</span><span class="sxs-lookup"><span data-stu-id="3ec37-124">Adding Learning Seat LMS from the gallery</span></span>
<span data-ttu-id="3ec37-125">Při konfiguraci integrace Learning stanici pro správu vzdělávacího procesu do služby Azure AD potřebujete přidat Learning stanici pro správu vzdělávacího procesu z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="3ec37-125">To configure the integration of Learning Seat LMS into Azure AD, you need to add Learning Seat LMS from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3ec37-126">**Pokud chcete přidat stanici Learning vzdělávacího procesu z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3ec37-126">**To add Learning Seat LMS from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3ec37-127">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3ec37-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3ec37-129">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3ec37-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3ec37-130">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3ec37-130">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="3ec37-132">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3ec37-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="3ec37-134">Do vyhledávacího pole zadejte **Learning stanici pro správu vzdělávacího procesu**.</span><span class="sxs-lookup"><span data-stu-id="3ec37-134">In the search box, type **Learning Seat LMS**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_search.png)

5. <span data-ttu-id="3ec37-136">Na panelu výsledků vyberte **Learning stanici pro správu vzdělávacího procesu**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3ec37-136">In the results panel, select **Learning Seat LMS**, and then click **Add** button to add the application.</span></span>


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3ec37-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3ec37-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3ec37-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Learning stanici pro správu vzdělávacího procesu na základě testovací uživatele, nazývá "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="3ec37-138">In this section, you configure and test Azure AD single sign-on with Learning Seat LMS based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3ec37-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Learning stanici pro správu vzdělávacího procesu je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3ec37-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Learning Seat LMS is to a user in Azure AD.</span></span> <span data-ttu-id="3ec37-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Learning stanici pro správu vzdělávacího procesu musí navázat.</span><span class="sxs-lookup"><span data-stu-id="3ec37-140">In other words, a link relationship between an Azure AD user and the related user in Learning Seat LMS needs to be established.</span></span>

<span data-ttu-id="3ec37-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Learning stanici pro správu vzdělávacího procesu.</span><span class="sxs-lookup"><span data-stu-id="3ec37-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Learning Seat LMS.</span></span>

<span data-ttu-id="3ec37-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Learning stanici pro správu vzdělávacího procesu, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="3ec37-142">To configure and test Azure AD single sign-on with Learning Seat LMS, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3ec37-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="3ec37-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3ec37-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3ec37-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3ec37-145">**[Vytvoření zkušebního uživatele Learning stanici pro správu vzdělávacího procesu](#creating-a-learnconnect-test-user)**  – Pokud chcete mít protějšek Britta Simon v Learning stanici pro správu vzdělávacího procesu propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="3ec37-145">**[Creating a Learning Seat LMS test user](#creating-a-learnconnect-test-user)** - to have a counterpart of Britta Simon in Learning Seat LMS that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3ec37-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3ec37-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3ec37-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3ec37-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3ec37-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3ec37-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3ec37-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci pro správu vzdělávacího procesu Learning stanici.</span><span class="sxs-lookup"><span data-stu-id="3ec37-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Learning Seat LMS application.</span></span>

<span data-ttu-id="3ec37-150">**Ke konfiguraci Azure AD jednotné přihlašování s Learning stanici pro správu vzdělávacího procesu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3ec37-150">**To configure Azure AD single sign-on with Learning Seat LMS, perform the following steps:**</span></span>

1. <span data-ttu-id="3ec37-151">Na portálu Azure na **Learning stanici pro správu vzdělávacího procesu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3ec37-151">In the Azure portal, on the **Learning Seat LMS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="3ec37-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3ec37-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_samlbase.png)

3. <span data-ttu-id="3ec37-155">Na **Learning stanici pro správu vzdělávacího procesu domény a adresy URL** část, proveďte následující kroky, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="3ec37-155">On the **Learning Seat LMS Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_url.png)

    <span data-ttu-id="3ec37-157">a.</span><span class="sxs-lookup"><span data-stu-id="3ec37-157">a.</span></span> <span data-ttu-id="3ec37-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.learningseatlms.com`</span><span class="sxs-lookup"><span data-stu-id="3ec37-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.learningseatlms.com`</span></span>

    <span data-ttu-id="3ec37-159">b.</span><span class="sxs-lookup"><span data-stu-id="3ec37-159">b.</span></span> <span data-ttu-id="3ec37-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.learningseatlms.com/Account/AssertionConsumerService`</span><span class="sxs-lookup"><span data-stu-id="3ec37-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.learningseatlms.com/Account/AssertionConsumerService`</span></span>

4. <span data-ttu-id="3ec37-161">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="3ec37-161">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_url2.png)

    <span data-ttu-id="3ec37-163">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.learningseatlms.com`</span><span class="sxs-lookup"><span data-stu-id="3ec37-163">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.learningseatlms.com`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="3ec37-164">Tyto hodnoty nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3ec37-164">These values are not the real values.</span></span> <span data-ttu-id="3ec37-165">Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="3ec37-165">Update these values with the actual Identifier, Reply URL and Sign-On URL.</span></span> <span data-ttu-id="3ec37-166">Obraťte se na [tým podpory Learning stanici](http://help.learningseatlms.com/help) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="3ec37-166">Contact [Learning Seat support team](http://help.learningseatlms.com/help) to get these values.</span></span> 

5. <span data-ttu-id="3ec37-167">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="3ec37-167">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_certificate.png) 

6. <span data-ttu-id="3ec37-169">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3ec37-169">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnconnect-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="3ec37-171">Konfigurace jednotného přihlašování na **Learning stanici pro správu vzdělávacího procesu** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory Learning stanici](http://help.learningseatlms.com/help).</span><span class="sxs-lookup"><span data-stu-id="3ec37-171">To configure single sign-on on **Learning Seat LMS** side, you need to send the downloaded **Metadata XML** to [Learning Seat support team](http://help.learningseatlms.com/help).</span></span>

> [!TIP]
> <span data-ttu-id="3ec37-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="3ec37-172">You can now read a concise version of these instructions inside the [Azure  portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3ec37-173">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="3ec37-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3ec37-174">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD](https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3ec37-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation](https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3ec37-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3ec37-175">Creating an Azure AD test user</span></span>

<span data-ttu-id="3ec37-176">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3ec37-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="3ec37-178">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3ec37-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3ec37-179">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3ec37-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnconnect-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3ec37-181">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3ec37-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnconnect-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3ec37-183">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3ec37-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnconnect-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3ec37-185">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3ec37-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-learnconnect-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3ec37-187">a.</span><span class="sxs-lookup"><span data-stu-id="3ec37-187">a.</span></span> <span data-ttu-id="3ec37-188">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3ec37-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3ec37-189">b.</span><span class="sxs-lookup"><span data-stu-id="3ec37-189">b.</span></span> <span data-ttu-id="3ec37-190">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3ec37-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3ec37-191">c.</span><span class="sxs-lookup"><span data-stu-id="3ec37-191">c.</span></span> <span data-ttu-id="3ec37-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="3ec37-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3ec37-193">d.</span><span class="sxs-lookup"><span data-stu-id="3ec37-193">d.</span></span> <span data-ttu-id="3ec37-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3ec37-194">Click **Create**.</span></span>
 
### <a name="creating-a-learning-seat-lms-test-user"></a><span data-ttu-id="3ec37-195">Vytvoření zkušebního uživatele Learning stanici pro správu vzdělávacího procesu</span><span class="sxs-lookup"><span data-stu-id="3ec37-195">Creating a Learning Seat LMS test user</span></span>

<span data-ttu-id="3ec37-196">V této části vytvoříte uživatele volal Britta Simon v Learning stanici pro správu vzdělávacího procesu.</span><span class="sxs-lookup"><span data-stu-id="3ec37-196">In this section, you create a user called Britta Simon in Learning Seat LMS.</span></span> <span data-ttu-id="3ec37-197">Obraťte se na [tým podpory Learning stanici](http://help.learningseatlms.com/help) všechny informace uživatele Chcete-li přidat uživatele v aplikaci pro správu vzdělávacího procesu Learning stanici.</span><span class="sxs-lookup"><span data-stu-id="3ec37-197">Contact [Learning Seat support team](http://help.learningseatlms.com/help) with all the user information to add the users in the Learning Seat LMS application.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3ec37-198">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3ec37-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3ec37-199">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Learning stanici pro správu vzdělávacího procesu.</span><span class="sxs-lookup"><span data-stu-id="3ec37-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Learning Seat LMS.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="3ec37-201">**Pokud chcete přiřadit Britta Simon Learning stanici pro správu vzdělávacího procesu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3ec37-201">**To assign Britta Simon to Learning Seat LMS, perform the following steps:**</span></span>

1. <span data-ttu-id="3ec37-202">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3ec37-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3ec37-204">V seznamu aplikací vyberte **Learning stanici pro správu vzdělávacího procesu**.</span><span class="sxs-lookup"><span data-stu-id="3ec37-204">In the applications list, select **Learning Seat LMS**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-learnconnect-tutorial/tutorial_learnconnect_app.png) 

3. <span data-ttu-id="3ec37-206">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3ec37-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="3ec37-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3ec37-208">Click **Add** button.</span></span> <span data-ttu-id="3ec37-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3ec37-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="3ec37-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3ec37-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3ec37-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3ec37-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3ec37-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3ec37-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3ec37-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3ec37-214">Testing single sign-on</span></span>

<span data-ttu-id="3ec37-215">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3ec37-215">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span> 

<span data-ttu-id="3ec37-216">Klikněte na dlaždici Learning stanici pro správu vzdělávacího procesu na přístupovém panelu, můžete se být automaticky přihlášení k aplikaci pro správu vzdělávacího procesu Learning stanici.</span><span class="sxs-lookup"><span data-stu-id="3ec37-216">Click the Learning Seat LMS tile in the Access Panel, you will be automatically signed-on to your Learning Seat LMS application.</span></span> <span data-ttu-id="3ec37-217">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="3ec37-217">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3ec37-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3ec37-218">Additional resources</span></span>

* [<span data-ttu-id="3ec37-219">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3ec37-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3ec37-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3ec37-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learnconnect-tutorial/tutorial_general_203.png

