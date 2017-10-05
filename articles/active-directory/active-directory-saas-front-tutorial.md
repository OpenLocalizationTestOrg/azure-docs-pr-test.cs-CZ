---
title: "Kurz: Azure Active Directory integrace s přední | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a popředí."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 88270b6d-2571-434a-b139-b6ccc3a2b19f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: d936bc50a66ac2a3c17038ff08351edf9902c99f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-front"></a><span data-ttu-id="1ee2a-103">Kurz: Azure Active Directory integrace s popředí</span><span class="sxs-lookup"><span data-stu-id="1ee2a-103">Tutorial: Azure Active Directory integration with Front</span></span>

<span data-ttu-id="1ee2a-104">V tomto kurzu zjistěte, jak integrovat před službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1ee2a-104">In this tutorial, you learn how to integrate Front with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1ee2a-105">Integrace přední s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="1ee2a-105">Integrating Front with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="1ee2a-106">Můžete ovládat ve službě Azure AD, který má přístup do popředí.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-106">You can control in Azure AD who has access to Front.</span></span>
- <span data-ttu-id="1ee2a-107">Můžete povolit uživatelům, aby automaticky získat přihlášeného do popředí (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-107">You can enable your users to automatically get signed-on to Front (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="1ee2a-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="1ee2a-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1ee2a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1ee2a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1ee2a-110">Prerequisites</span></span>

<span data-ttu-id="1ee2a-111">Konfigurace integrace Azure AD s přední, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="1ee2a-111">To configure Azure AD integration with Front, you need the following items:</span></span>

- <span data-ttu-id="1ee2a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="1ee2a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1ee2a-113">Popředí jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="1ee2a-113">A Front single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1ee2a-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1ee2a-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="1ee2a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1ee2a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1ee2a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1ee2a-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1ee2a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="1ee2a-118">Scenario description</span></span>
<span data-ttu-id="1ee2a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1ee2a-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="1ee2a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1ee2a-121">Přidání přední z Galerie</span><span class="sxs-lookup"><span data-stu-id="1ee2a-121">Adding Front from the gallery</span></span>
2. <span data-ttu-id="1ee2a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1ee2a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-front-from-the-gallery"></a><span data-ttu-id="1ee2a-123">Přidání přední z Galerie</span><span class="sxs-lookup"><span data-stu-id="1ee2a-123">Adding Front from the gallery</span></span>
<span data-ttu-id="1ee2a-124">Při konfiguraci integrace přední do služby Azure AD potřebujete přidat před z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-124">To configure the integration of Front into Azure AD, you need to add Front from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="1ee2a-125">**Pokud chcete přidat před z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1ee2a-125">**To add Front from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="1ee2a-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="1ee2a-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="1ee2a-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="1ee2a-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="1ee2a-133">Do vyhledávacího pole zadejte **Front**, vyberte **Front** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-133">In the search box, type **Front**, select **Front** from result panel then click **Add** button to add the application.</span></span>

    ![Přední v seznamu výsledků](./media/active-directory-saas-front-tutorial/tutorial_front_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="1ee2a-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1ee2a-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="1ee2a-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s přední podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="1ee2a-136">In this section, you configure and test Azure AD single sign-on with Front based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1ee2a-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějšku vpředu je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Front is to a user in Azure AD.</span></span> <span data-ttu-id="1ee2a-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské vpředu musí navázat.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-138">In other words, a link relationship between an Azure AD user and the related user in Front needs to be established.</span></span>

<span data-ttu-id="1ee2a-139">Vpředu, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-139">In Front, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="1ee2a-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s přední, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="1ee2a-140">To configure and test Azure AD single sign-on with Front, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="1ee2a-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="1ee2a-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1ee2a-143">**[Vytvoření zkušebního uživatele před](#create-a-front-test-user)**  – Pokud chcete mít protějškem Britta Simon vpředu propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-143">**[Create a Front test user](#create-a-front-test-user)** - to have a counterpart of Britta Simon in Front that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="1ee2a-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1ee2a-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="1ee2a-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1ee2a-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="1ee2a-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci popředí.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Front application.</span></span>

<span data-ttu-id="1ee2a-148">**Ke konfiguraci Azure AD jednotné přihlašování s přední, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1ee2a-148">**To configure Azure AD single sign-on with Front, perform the following steps:**</span></span>

1. <span data-ttu-id="1ee2a-149">Na portálu Azure na **Front** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-149">In the Azure portal, on the **Front** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="1ee2a-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-front-tutorial/tutorial_front_samlbase.png)

3. <span data-ttu-id="1ee2a-153">Na **Front domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="1ee2a-153">On the **Front Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-front-tutorial/tutorial_front_url1.png)

    <span data-ttu-id="1ee2a-155">a.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-155">a.</span></span> <span data-ttu-id="1ee2a-156">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.frontapp.com`</span><span class="sxs-lookup"><span data-stu-id="1ee2a-156">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.frontapp.com`</span></span>

    <span data-ttu-id="1ee2a-157">b.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-157">b.</span></span> <span data-ttu-id="1ee2a-158">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.frontapp.com/sso/saml/callback`</span><span class="sxs-lookup"><span data-stu-id="1ee2a-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyname>.frontapp.com/sso/saml/callback`</span></span>

4. <span data-ttu-id="1ee2a-159">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="1ee2a-159">Check **Show advanced URL settings**, If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-front-tutorial/tutorial_front_url2.png)

    <span data-ttu-id="1ee2a-161">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.frontapp.com`</span><span class="sxs-lookup"><span data-stu-id="1ee2a-161">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.frontapp.com`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="1ee2a-162">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-162">These values are not real.</span></span> <span data-ttu-id="1ee2a-163">Tyto hodnoty aktualizovat s skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL, které jsou vysvětleny později v kurzu nebo kontaktujte [tým podpory Front klienta](mailto:support@frontapp.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-163">Update these values with the actual Identifier, Reply URL, and Sign-On URL which are explained later in tutorial or contact [Front Client support team](mailto:support@frontapp.com) to get these values.</span></span> 

5. <span data-ttu-id="1ee2a-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-front-tutorial/tutorial_front_certificate.png) 

6. <span data-ttu-id="1ee2a-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-front-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="1ee2a-168">Na **Front konfigurace** klikněte na tlačítko **nakonfigurovat Front** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-168">On the **Front Configuration** section, click **Configure Front** to open **Configure sign-on** window.</span></span> <span data-ttu-id="1ee2a-169">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="1ee2a-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-front-tutorial/tutorial_front_configure.png) 

8. <span data-ttu-id="1ee2a-171">Přihlášení ke klientovi přední jako správce.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-171">Sign-on to your Front tenant as an administrator.</span></span>

9. <span data-ttu-id="1ee2a-172">Přejděte na **nastavení (ozubené kolo ikona v dolní části na levém bočním panelu) > Předvolby**.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-172">Go to **Settings (cog icon at the bottom of the left sidebar) > Preferences**.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-front-tutorial/tutorial_front_000.png)

10. <span data-ttu-id="1ee2a-174">Klikněte na tlačítko **jednotné přihlašování** odkaz.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-174">Click **Single Sign On** link.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-front-tutorial/tutorial_front_001.png)

11. <span data-ttu-id="1ee2a-176">Vyberte **SAML** v rozevíracím seznamu **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-176">Select **SAML** in the drop-down list of **Single Sign On**.</span></span>
   
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-front-tutorial/tutorial_front_002.png)

12. <span data-ttu-id="1ee2a-178">V **vstupní bod** textbox vložte hodnotu **jeden přihlašování adresa URL služby** z Průvodce konfigurací aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-178">In the **Entry Point** textbox put the value of **Single Sign-on Service URL** from Azure AD application configuration wizard.</span></span>
    
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-front-tutorial/tutorial_front_003.png)

13. <span data-ttu-id="1ee2a-180">Otevřete váš stažené **Certificate(Base64)** souboru v programu Poznámkový blok, zkopírujte obsah ho do schránky a vložte jej do **podpisového certifikátu** textové pole.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-180">Open your downloaded **Certificate(Base64)** file in notepad, copy the content of it into your clipboard, and then paste it to the **Signing certificate** textbox.</span></span>
    
    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-front-tutorial/tutorial_front_004.png)

14. <span data-ttu-id="1ee2a-182">Na **nastavení poskytovatele služby** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1ee2a-182">On the **Service provider settings** section, perform the following steps:</span></span>

    ![Konfigurace straně jediné přihlášení na aplikaci](./media/active-directory-saas-front-tutorial/tutorial_front_005.png)

    <span data-ttu-id="1ee2a-184">a.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-184">a.</span></span> <span data-ttu-id="1ee2a-185">Zkopírujte hodnotu **Entity ID** a vložte ji do **identifikátor** textového pole v **Front domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-185">Copy the value of **Entity ID** and paste it into the **Identifier** textbox in **Front Domain and URLs** section in Azure portal.</span></span>

    <span data-ttu-id="1ee2a-186">b.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-186">b.</span></span> <span data-ttu-id="1ee2a-187">Zkopírujte hodnotu **adresa URL služby ACS** a vložte ji do **přihlašovací adresa URL** textového pole v **Front domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-187">Copy the value of **ACS URL** and paste it into the **Sign-on URL** textbox in **Front Domain and URLs** section in Azure portal.</span></span>
    
15. <span data-ttu-id="1ee2a-188">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-188">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="1ee2a-189">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="1ee2a-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="1ee2a-190">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="1ee2a-191">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1ee2a-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="1ee2a-192">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="1ee2a-192">Create an Azure AD test user</span></span>

<span data-ttu-id="1ee2a-193">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="1ee2a-195">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1ee2a-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="1ee2a-196">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-196">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-front-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="1ee2a-198">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-198">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-front-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="1ee2a-200">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-200">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-front-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="1ee2a-202">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1ee2a-202">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-front-tutorial/create_aaduser_04.png)

    <span data-ttu-id="1ee2a-204">a.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-204">a.</span></span> <span data-ttu-id="1ee2a-205">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-205">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1ee2a-206">b.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-206">b.</span></span> <span data-ttu-id="1ee2a-207">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-207">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="1ee2a-208">c.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-208">c.</span></span> <span data-ttu-id="1ee2a-209">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-209">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="1ee2a-210">d.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-210">d.</span></span> <span data-ttu-id="1ee2a-211">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-211">Click **Create**.</span></span>
 
### <a name="create-a-front-test-user"></a><span data-ttu-id="1ee2a-212">Vytvoření zkušebního uživatele před</span><span class="sxs-lookup"><span data-stu-id="1ee2a-212">Create a Front test user</span></span>

<span data-ttu-id="1ee2a-213">V této části vytvoříte uživatele volat Britta Simon vpředu.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-213">In this section, you create a user called Britta Simon in Front.</span></span> <span data-ttu-id="1ee2a-214">Práce s [tým podpory Front klienta](mailto:support@frontapp.com) přidat uživatele do přední platformy.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-214">Work with [Front Client support team](mailto:support@frontapp.com) to add the users in the Front platform.</span></span> <span data-ttu-id="1ee2a-215">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-215">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="1ee2a-216">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="1ee2a-216">Assign the Azure AD test user</span></span>

<span data-ttu-id="1ee2a-217">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu do popředí.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-217">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Front.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="1ee2a-219">**Přiřadit Britta Simon dopředu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1ee2a-219">**To assign Britta Simon to Front, perform the following steps:**</span></span>

1. <span data-ttu-id="1ee2a-220">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-220">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="1ee2a-222">V seznamu aplikací vyberte **Front**.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-222">In the applications list, select **Front**.</span></span>

    ![V seznamu aplikací na přední odkaz](./media/active-directory-saas-front-tutorial/tutorial_front_app.png)  

3. <span data-ttu-id="1ee2a-224">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-224">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="1ee2a-226">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-226">Click **Add** button.</span></span> <span data-ttu-id="1ee2a-227">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="1ee2a-229">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-229">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="1ee2a-230">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1ee2a-231">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="1ee2a-232">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1ee2a-232">Test single sign-on</span></span>

<span data-ttu-id="1ee2a-233">Cílem této části je pro testování vaší Azure AD SSOconfiguration pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-233">The objective of this section is to test your Azure AD SSOconfiguration using the Access Panel.</span></span>

<span data-ttu-id="1ee2a-234">Když kliknete na dlaždici přední na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci před.</span><span class="sxs-lookup"><span data-stu-id="1ee2a-234">When you click the Front tile in the Access Panel, you should get automatically signed-on to your Front application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="1ee2a-235">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1ee2a-235">Additional resources</span></span>

* [<span data-ttu-id="1ee2a-236">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1ee2a-236">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1ee2a-237">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1ee2a-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-front-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-front-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-front-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-front-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-front-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-front-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-front-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-front-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-front-tutorial/tutorial_general_203.png

