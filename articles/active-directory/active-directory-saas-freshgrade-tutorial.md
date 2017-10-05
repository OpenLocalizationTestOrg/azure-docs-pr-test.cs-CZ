---
title: 'Kurz: Azure Active Directory integrace s FreshGrade | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a FreshGrade."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1055bba6-f4df-462e-bc9b-1ad5ada0f638
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 3ff3e5aab679f8ee610c98f8a4089308adcce48f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshgrade"></a><span data-ttu-id="7c07e-103">Kurz: Azure Active Directory integrace s FreshGrade</span><span class="sxs-lookup"><span data-stu-id="7c07e-103">Tutorial: Azure Active Directory integration with FreshGrade</span></span>

<span data-ttu-id="7c07e-104">V tomto kurzu zjistěte, jak integrovat FreshGrade s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7c07e-104">In this tutorial, you learn how to integrate FreshGrade with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7c07e-105">Integrace FreshGrade s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="7c07e-105">Integrating FreshGrade with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7c07e-106">Můžete řídit ve službě Azure AD, který má přístup k FreshGrade</span><span class="sxs-lookup"><span data-stu-id="7c07e-106">You can control in Azure AD who has access to FreshGrade</span></span>
- <span data-ttu-id="7c07e-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k FreshGrade (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="7c07e-107">You can enable your users to automatically get signed-on to FreshGrade (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7c07e-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7c07e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="7c07e-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7c07e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7c07e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7c07e-110">Prerequisites</span></span>

<span data-ttu-id="7c07e-111">Konfigurace integrace Azure AD s FreshGrade, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="7c07e-111">To configure Azure AD integration with FreshGrade, you need the following items:</span></span>

- <span data-ttu-id="7c07e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="7c07e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7c07e-113">FreshGrade jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="7c07e-113">A FreshGrade single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7c07e-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="7c07e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7c07e-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="7c07e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7c07e-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="7c07e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7c07e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7c07e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7c07e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="7c07e-118">Scenario description</span></span>
<span data-ttu-id="7c07e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="7c07e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7c07e-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="7c07e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7c07e-121">Přidání FreshGrade z Galerie</span><span class="sxs-lookup"><span data-stu-id="7c07e-121">Adding FreshGrade from the gallery</span></span>
2. <span data-ttu-id="7c07e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7c07e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-freshgrade-from-the-gallery"></a><span data-ttu-id="7c07e-123">Přidání FreshGrade z Galerie</span><span class="sxs-lookup"><span data-stu-id="7c07e-123">Adding FreshGrade from the gallery</span></span>
<span data-ttu-id="7c07e-124">Při konfiguraci integrace FreshGrade do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS FreshGrade z galerie.</span><span class="sxs-lookup"><span data-stu-id="7c07e-124">To configure the integration of FreshGrade into Azure AD, you need to add FreshGrade from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7c07e-125">**Pokud chcete přidat FreshGrade z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7c07e-125">**To add FreshGrade from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7c07e-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7c07e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7c07e-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="7c07e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7c07e-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7c07e-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="7c07e-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7c07e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="7c07e-133">Do vyhledávacího pole zadejte **FreshGrade**.</span><span class="sxs-lookup"><span data-stu-id="7c07e-133">In the search box, type **FreshGrade**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_search.png)

5. <span data-ttu-id="7c07e-135">Na panelu výsledků vyberte **FreshGrade**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7c07e-135">In the results panel, select **FreshGrade**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7c07e-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7c07e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7c07e-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s FreshGrade podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="7c07e-138">In this section, you configure and test Azure AD single sign-on with FreshGrade based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7c07e-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v FreshGrade je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7c07e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in FreshGrade is to a user in Azure AD.</span></span> <span data-ttu-id="7c07e-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v FreshGrade musí navázat.</span><span class="sxs-lookup"><span data-stu-id="7c07e-140">In other words, a link relationship between an Azure AD user and the related user in FreshGrade needs to be established.</span></span>

<span data-ttu-id="7c07e-141">V FreshGrade, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="7c07e-141">In FreshGrade, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="7c07e-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s FreshGrade, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="7c07e-142">To configure and test Azure AD single sign-on with FreshGrade, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7c07e-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="7c07e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7c07e-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7c07e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7c07e-145">**[Vytvoření zkušebního uživatele FreshGrade](#creating-a-freshgrade-test-user)**  – Pokud chcete mít protějšek Britta Simon v FreshGrade propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="7c07e-145">**[Creating a FreshGrade test user](#creating-a-freshgrade-test-user)** - to have a counterpart of Britta Simon in FreshGrade that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7c07e-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7c07e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7c07e-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7c07e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7c07e-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7c07e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7c07e-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci FreshGrade.</span><span class="sxs-lookup"><span data-stu-id="7c07e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your FreshGrade application.</span></span>

<span data-ttu-id="7c07e-150">**Ke konfiguraci Azure AD jednotné přihlašování s FreshGrade, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7c07e-150">**To configure Azure AD single sign-on with FreshGrade, perform the following steps:**</span></span>

1. <span data-ttu-id="7c07e-151">Na portálu Azure na **FreshGrade** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="7c07e-151">In the Azure portal, on the **FreshGrade** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="7c07e-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7c07e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_samlbase.png)

3. <span data-ttu-id="7c07e-155">Na **FreshGrade domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7c07e-155">On the **FreshGrade Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_url.png)

    <span data-ttu-id="7c07e-157">a.</span><span class="sxs-lookup"><span data-stu-id="7c07e-157">a.</span></span> <span data-ttu-id="7c07e-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="7c07e-158">In the **Sign-on URL** textbox, type a URL using the following patterns:</span></span> 
      | |
      |--|
      | `https://<subdomain>.freshgrade.com/login` |    
      | `https://<subdomain>.onboarding.freshgrade.com/login` |

    <span data-ttu-id="7c07e-159">b.</span><span class="sxs-lookup"><span data-stu-id="7c07e-159">b.</span></span> <span data-ttu-id="7c07e-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="7c07e-160">In the **Identifier** textbox, type a URL using the following patterns:</span></span> 
      | |
      |--|
      | `https://login.onboarding.freshgrade.com:443/saml/metadata/alias/<instancename>` |      
      | `https://login.freshgrade.com:443/saml/metadata/alias/<instancename>` |

    > [!NOTE] 
    > <span data-ttu-id="7c07e-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="7c07e-161">These values are not real.</span></span> <span data-ttu-id="7c07e-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="7c07e-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="7c07e-163">Obraťte se na [tým podpory FreshGrade klienta](mailTo:support@freshgrade.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="7c07e-163">Contact [FreshGrade Client support team](mailTo:support@freshgrade.com) to get these values.</span></span> 
 


4. <span data-ttu-id="7c07e-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="7c07e-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_certificate.png) 

5. <span data-ttu-id="7c07e-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7c07e-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7c07e-168">Na **FreshGrade konfigurace** klikněte na tlačítko **konfigurace FreshGrade** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="7c07e-168">On the **FreshGrade Configuration** section, click **Configure FreshGrade** to open **Configure sign-on** window.</span></span> <span data-ttu-id="7c07e-169">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="7c07e-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_configure.png) 

7. <span data-ttu-id="7c07e-171">Ke generování **Metadata** adresu url, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7c07e-171">To generate the **Metadata** url, perform the following steps:</span></span>

    <span data-ttu-id="7c07e-172">a.</span><span class="sxs-lookup"><span data-stu-id="7c07e-172">a.</span></span> <span data-ttu-id="7c07e-173">Klikněte na tlačítko **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7c07e-173">Click **App registrations**.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_appregistrations.png)
   
    <span data-ttu-id="7c07e-175">b.</span><span class="sxs-lookup"><span data-stu-id="7c07e-175">b.</span></span> <span data-ttu-id="7c07e-176">Klikněte na tlačítko **koncové body** otevřete **koncové body** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7c07e-176">Click **Endpoints** to open **Endpoints** dialog box.</span></span>  
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_endpointicon.png)

    <span data-ttu-id="7c07e-178">c.</span><span class="sxs-lookup"><span data-stu-id="7c07e-178">c.</span></span> <span data-ttu-id="7c07e-179">Klikněte na tlačítko Kopírovat kopírování **dokument FEDERAČNÍCH METADAT** adresy url a vložte do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="7c07e-179">Click the copy button to copy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_endpoint.png)
     
    <span data-ttu-id="7c07e-181">d.</span><span class="sxs-lookup"><span data-stu-id="7c07e-181">d.</span></span> <span data-ttu-id="7c07e-182">Nyní přejděte na stránku vlastností **FreshGrade** a zkopírujte **Id aplikace** pomocí **kopie** tlačítko a vložte do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="7c07e-182">Now go to the property page of **FreshGrade** and copy the **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_appid.png)

    <span data-ttu-id="7c07e-184">e.</span><span class="sxs-lookup"><span data-stu-id="7c07e-184">e.</span></span> <span data-ttu-id="7c07e-185">Vygenerovat **adresu URL metadat** pomocí následujícího vzorce:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="7c07e-185">Generate the **Metadata URL** using the following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>

8. <span data-ttu-id="7c07e-186">Konfigurace jednotného přihlašování na **FreshGrade** straně, budete muset odeslat **adresu URL metadat** a **SAML jeden přihlašování adresa URL služby** k [FreshGrade tým podpory ](mailTo:support@freshgrade.com).</span><span class="sxs-lookup"><span data-stu-id="7c07e-186">To configure single sign-on on **FreshGrade** side, you need to send the **Metadata URL** and **SAML Single Sign-On Service URL** to [FreshGrade support team](mailTo:support@freshgrade.com).</span></span> <span data-ttu-id="7c07e-187">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="7c07e-187">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="7c07e-188">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="7c07e-188">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="7c07e-189">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="7c07e-189">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="7c07e-190">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7c07e-190">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7c07e-191">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="7c07e-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="7c07e-192">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7c07e-192">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="7c07e-194">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7c07e-194">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7c07e-195">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7c07e-195">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshgrade-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7c07e-197">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="7c07e-197">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshgrade-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7c07e-199">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7c07e-199">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshgrade-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7c07e-201">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7c07e-201">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-freshgrade-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7c07e-203">a.</span><span class="sxs-lookup"><span data-stu-id="7c07e-203">a.</span></span> <span data-ttu-id="7c07e-204">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7c07e-204">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7c07e-205">b.</span><span class="sxs-lookup"><span data-stu-id="7c07e-205">b.</span></span> <span data-ttu-id="7c07e-206">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7c07e-206">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7c07e-207">c.</span><span class="sxs-lookup"><span data-stu-id="7c07e-207">c.</span></span> <span data-ttu-id="7c07e-208">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="7c07e-208">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="7c07e-209">d.</span><span class="sxs-lookup"><span data-stu-id="7c07e-209">d.</span></span> <span data-ttu-id="7c07e-210">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7c07e-210">Click **Create**.</span></span>
 
### <a name="creating-a-freshgrade-test-user"></a><span data-ttu-id="7c07e-211">Vytvoření zkušebního uživatele FreshGrade</span><span class="sxs-lookup"><span data-stu-id="7c07e-211">Creating a FreshGrade test user</span></span>

<span data-ttu-id="7c07e-212">V této části vytvoříte volal Britta Simon v FreshGrade uživatele.</span><span class="sxs-lookup"><span data-stu-id="7c07e-212">In this section, you create a user called Britta Simon in FreshGrade.</span></span> <span data-ttu-id="7c07e-213">Spojte se s [tým podpory FreshGrade](mailTo:support@freshgrade.com) přidat uživatele do FreshGrade platformy.</span><span class="sxs-lookup"><span data-stu-id="7c07e-213">Please work with [FreshGrade support team](mailTo:support@freshgrade.com) to add the users in the FreshGrade platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="7c07e-214">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="7c07e-214">Assigning the Azure AD test user</span></span>

<span data-ttu-id="7c07e-215">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu FreshGrade.</span><span class="sxs-lookup"><span data-stu-id="7c07e-215">In this section, you enable Britta Simon to use Azure single sign-on by granting access to FreshGrade.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="7c07e-217">**Pokud chcete přiřadit Britta Simon FreshGrade, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7c07e-217">**To assign Britta Simon to FreshGrade, perform the following steps:**</span></span>

1. <span data-ttu-id="7c07e-218">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7c07e-218">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="7c07e-220">V seznamu aplikací vyberte **FreshGrade**.</span><span class="sxs-lookup"><span data-stu-id="7c07e-220">In the applications list, select **FreshGrade**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_app.png) 

3. <span data-ttu-id="7c07e-222">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="7c07e-222">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="7c07e-224">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7c07e-224">Click **Add** button.</span></span> <span data-ttu-id="7c07e-225">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7c07e-225">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="7c07e-227">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="7c07e-227">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7c07e-228">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7c07e-228">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7c07e-229">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7c07e-229">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7c07e-230">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7c07e-230">Testing single sign-on</span></span>

<span data-ttu-id="7c07e-231">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="7c07e-231">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="7c07e-232">Když kliknete na dlaždici FreshGrade na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci FreshGrade.</span><span class="sxs-lookup"><span data-stu-id="7c07e-232">When you click the FreshGrade tile in the Access Panel, you should get automatically signed-on to your FreshGrade application.</span></span>
<span data-ttu-id="7c07e-233">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7c07e-233">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7c07e-234">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7c07e-234">Additional resources</span></span>

* [<span data-ttu-id="7c07e-235">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7c07e-235">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7c07e-236">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7c07e-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_203.png

