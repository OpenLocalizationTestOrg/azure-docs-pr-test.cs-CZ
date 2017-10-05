---
title: 'Kurz: Azure Active Directory integrace s RFPIO | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a RFPIO."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 87187076-7b50-4247-814f-f217b052703f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 26a8bb17dad5a01b401ce7f9b484f09822825cbf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a><span data-ttu-id="ad082-103">Kurz: Azure Active Directory integrace s RFPIO</span><span class="sxs-lookup"><span data-stu-id="ad082-103">Tutorial: Azure Active Directory integration with RFPIO</span></span>

<span data-ttu-id="ad082-104">V tomto kurzu zjistěte, jak integrovat RFPIO s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ad082-104">In this tutorial, you learn how to integrate RFPIO with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ad082-105">Integrace RFPIO s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ad082-105">Integrating RFPIO with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ad082-106">Můžete určovat, kdo ve službě Azure AD, který má přístup k RFPIO.</span><span class="sxs-lookup"><span data-stu-id="ad082-106">You can control who in Azure AD who has access to RFPIO.</span></span>
- <span data-ttu-id="ad082-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k RFPIO (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ad082-107">You can enable your users to automatically get signed-on to RFPIO (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="ad082-108">Můžete spravovat vaše účty v jednom centrálním místě – portál Azure.</span><span class="sxs-lookup"><span data-stu-id="ad082-108">You can manage your accounts in one central location--the Azure portal.</span></span>

<span data-ttu-id="ad082-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ad082-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ad082-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ad082-110">Prerequisites</span></span>

<span data-ttu-id="ad082-111">Konfigurace integrace Azure AD s RFPIO, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="ad082-111">To configure Azure AD integration with RFPIO, you need the following items:</span></span>

- <span data-ttu-id="ad082-112">Předplatné služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ad082-112">An Azure AD subscription.</span></span>
- <span data-ttu-id="ad082-113">RFPIO jednotného přihlašování na povolené předplatné.</span><span class="sxs-lookup"><span data-stu-id="ad082-113">A RFPIO single sign-on-enabled subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="ad082-114">Nedoporučujeme používat produkčním prostředí pro testování kroky v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ad082-114">We don't recommend that you use a production environment to test the steps in this tutorial.</span></span>

<span data-ttu-id="ad082-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle následujících doporučení:</span><span class="sxs-lookup"><span data-stu-id="ad082-115">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="ad082-116">Pokud to není nezbytné, nepoužívejte produkční prostředí.</span><span class="sxs-lookup"><span data-stu-id="ad082-116">Do not use your production environment unless it's necessary.</span></span>
- <span data-ttu-id="ad082-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ad082-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ad082-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="ad082-118">Scenario description</span></span>
<span data-ttu-id="ad082-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="ad082-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ad082-120">Scénář, který je popsané v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="ad082-120">The scenario that's outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ad082-121">Přidání RFPIO z galerie.</span><span class="sxs-lookup"><span data-stu-id="ad082-121">Adding RFPIO from the gallery.</span></span>
2. <span data-ttu-id="ad082-122">Konfigurace a testování Azure AD jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ad082-122">Configuring and testing Azure AD single sign-on.</span></span>

## <a name="add-rfpio-from-the-gallery"></a><span data-ttu-id="ad082-123">Přidat RFPIO z Galerie</span><span class="sxs-lookup"><span data-stu-id="ad082-123">Add RFPIO from the gallery</span></span>
<span data-ttu-id="ad082-124">Při konfiguraci integrace RFPIO do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS RFPIO z galerie.</span><span class="sxs-lookup"><span data-stu-id="ad082-124">To configure the integration of RFPIO into Azure AD, you need to add RFPIO from the gallery to your list of managed SaaS apps.</span></span>

### <a name="to-add-rfpio-from-the-gallery"></a><span data-ttu-id="ad082-125">Chcete-li přidat RFPIO z Galerie</span><span class="sxs-lookup"><span data-stu-id="ad082-125">To add RFPIO from the gallery</span></span>

1. <span data-ttu-id="ad082-126">V  **[portál Azure](https://portal.azure.com)**, na levém navigačním podokně, vyberte **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ad082-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation pane, select the **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ad082-128">Vyberte **podnikové aplikace, které**a potom vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ad082-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="ad082-130">Chcete-li přidat novou aplikaci, vyberte **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ad082-130">To add a new application, select the **New application** button on the top of dialog box.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="ad082-132">Do vyhledávacího pole zadejte **RFPIO**.</span><span class="sxs-lookup"><span data-stu-id="ad082-132">In the search box, type **RFPIO**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_search.png)

5. <span data-ttu-id="ad082-134">Na panelu výsledků vyberte **RFPIO**a pak vyberte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ad082-134">In the results panel, select **RFPIO**, and then select the **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="ad082-136">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ad082-136">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="ad082-137">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s RFPIO podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="ad082-137">In this section, you configure and test Azure AD single sign-on with RFPIO based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ad082-138">Azure AD pro jednotné přihlašování pro práci, musí vědět, co je vztah mezi uživatelem protějškem v RFPIO a uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ad082-138">For single sign-on to work, Azure AD needs to know what the relationship is between counterpart user in RFPIO and a user in Azure AD.</span></span> <span data-ttu-id="ad082-139">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v RFPIO musí navázat.</span><span class="sxs-lookup"><span data-stu-id="ad082-139">In other words, a link relationship between an Azure AD user and the related user in RFPIO needs to be established.</span></span>

<span data-ttu-id="ad082-140">V RFPIO, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="ad082-140">In RFPIO, assign the value of **user name** in Azure AD as the value of  **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ad082-141">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s RFPIO, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="ad082-141">To configure and test Azure AD single sign-on with RFPIO, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ad082-142">**[Konfigurovat Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**– Chcete-li povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="ad082-142">**[Configure Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**--to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ad082-143">**[Vytvořit testovací uživatele Azure AD](#creating-an-azure-ad-test-user)**– Azure AD jednotné přihlašování s Britta Simon otestovat.</span><span class="sxs-lookup"><span data-stu-id="ad082-143">**[Create an Azure AD test user](#creating-an-azure-ad-test-user)**-- to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ad082-144">**[Vytvoření zkušebního uživatele RFPIO](#creating-a-rfpio-test-user)**  – tak, aby měl protějšek Britta Simon v RFPIO propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="ad082-144">**[Create a RFPIO test user](#creating-a-rfpio-test-user)** --to have a counterpart of Britta Simon in RFPIO that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ad082-145">**[Přiřadit testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**– Chcete-li povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ad082-145">**[Assign the Azure AD test user](#assigning-the-azure-ad-test-user)**--to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ad082-146">**[Test jednotného přihlašování](#testing-single-sign-on)**  – Chcete-li ověřit, pokud konfigurace fungovat.</span><span class="sxs-lookup"><span data-stu-id="ad082-146">**[Test Single Sign-On](#testing-single-sign-on)** --to verify if the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="ad082-147">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="ad082-147">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="ad082-148">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci RFPIO.</span><span class="sxs-lookup"><span data-stu-id="ad082-148">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your RFPIO application.</span></span>

<span data-ttu-id="ad082-149">**Ke konfiguraci Azure AD jednotné přihlašování s RFPIO, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ad082-149">**To configure Azure AD single sign-on with RFPIO, perform the following steps:**</span></span>

1. <span data-ttu-id="ad082-150">Na portálu Azure na **RFPIO** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="ad082-150">In the Azure portal, on the **RFPIO** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="ad082-152">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ad082-152">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_samlbase.png)

3. <span data-ttu-id="ad082-154">Na **RFPIO domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="ad082-154">On the **RFPIO Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url.png)

    <span data-ttu-id="ad082-156">a.</span><span class="sxs-lookup"><span data-stu-id="ad082-156">a.</span></span> <span data-ttu-id="ad082-157">V **identifikátor** textovému poli, zadejte adresu URL:`https://www.rfpio.com`</span><span class="sxs-lookup"><span data-stu-id="ad082-157">In the **Identifier** textbox, type the URL: `https://www.rfpio.com`</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url1.png)

    <span data-ttu-id="ad082-159">b.</span><span class="sxs-lookup"><span data-stu-id="ad082-159">b.</span></span> <span data-ttu-id="ad082-160">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="ad082-160">Check **Show advanced URL settings**.</span></span>

    <span data-ttu-id="ad082-161">c.</span><span class="sxs-lookup"><span data-stu-id="ad082-161">c.</span></span> <span data-ttu-id="ad082-162">V **předávání stavu** textového pole zadejte hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="ad082-162">In the **Relay State** textbox enter a string value.</span></span> <span data-ttu-id="ad082-163">Obraťte se na [tým podpory RFPIO](https://www.rfpio.com/contact/) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ad082-163">Contact [RFPIO support team](https://www.rfpio.com/contact/) to get this value.</span></span> 

4. <span data-ttu-id="ad082-164">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="ad082-164">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="ad082-165">Pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="ad082-165">If you wish to configure the application in **SP** initiated mode:</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url2.png)

    <span data-ttu-id="ad082-167">V **přihlásit na adrese URL** textovému poli, zadejte adresu URL:`https://www.app.rfpio.com`</span><span class="sxs-lookup"><span data-stu-id="ad082-167">In the **Sign on URL** textbox, type the URL: `https://www.app.rfpio.com`</span></span>

5. <span data-ttu-id="ad082-168">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="ad082-168">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_certificate.png) 

6. <span data-ttu-id="ad082-170">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ad082-170">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="ad082-172">V okně prohlížeče jiný web, přihlášení, které **RFPIO** webu jako správce.</span><span class="sxs-lookup"><span data-stu-id="ad082-172">In a different web browser window, login to the **RFPIO** website as an administrator.</span></span>

8. <span data-ttu-id="ad082-173">Klikněte na rozevírací nabídce dolního rohu.</span><span class="sxs-lookup"><span data-stu-id="ad082-173">Click on the bottom left corner dropdown.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app1.png)

9. <span data-ttu-id="ad082-175">Klikněte na **nastavení organizace**.</span><span class="sxs-lookup"><span data-stu-id="ad082-175">Click on the **Organization Settings**.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app2.png)

10. <span data-ttu-id="ad082-177">Klikněte na **funkce a integrace**.</span><span class="sxs-lookup"><span data-stu-id="ad082-177">Click on the **FEATURES & INTEGRATION**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app4.png)

11. <span data-ttu-id="ad082-179">V **Konfigurace jednotného přihlašování SAML** klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="ad082-179">In the **SAML SSO Configuration** Click **Edit**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app3.png)

12. <span data-ttu-id="ad082-181">V této části proveďte následující akce:</span><span class="sxs-lookup"><span data-stu-id="ad082-181">In this Section perform following actions:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app5.png)
    
    <span data-ttu-id="ad082-183">a.</span><span class="sxs-lookup"><span data-stu-id="ad082-183">a.</span></span> <span data-ttu-id="ad082-184">Zkopírujte obsah **stáhnout soubor XML s metadaty** a vložte ji do **konfigurace identity** pole.</span><span class="sxs-lookup"><span data-stu-id="ad082-184">Copy the content of the **Downloaded Metadata XML** and paste it into the **identity configuration** field.</span></span>

    > [!NOTE]
    ><span data-ttu-id="ad082-185">Zkopírujte obsah stáhnout **soubor XML s metadaty** použití **Poznámkový blok ++** nebo správné **editoru XML**.</span><span class="sxs-lookup"><span data-stu-id="ad082-185">To copy the content of downloaded **Metadata XML** Use **Notepad++** or proper **XML Editor**.</span></span> 

    <span data-ttu-id="ad082-186">b.</span><span class="sxs-lookup"><span data-stu-id="ad082-186">b.</span></span> <span data-ttu-id="ad082-187">Klikněte na tlačítko **ověření**.</span><span class="sxs-lookup"><span data-stu-id="ad082-187">Click **Validate**.</span></span>

    <span data-ttu-id="ad082-188">c.</span><span class="sxs-lookup"><span data-stu-id="ad082-188">c.</span></span> <span data-ttu-id="ad082-189">Po kliknutí na **ověření**, podle **SAML(Enabled)** na on.</span><span class="sxs-lookup"><span data-stu-id="ad082-189">After Clicking **Validate**, Flip **SAML(Enabled)** to on.</span></span>

    <span data-ttu-id="ad082-190">d.</span><span class="sxs-lookup"><span data-stu-id="ad082-190">d.</span></span> <span data-ttu-id="ad082-191">Klikněte na tlačítko **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="ad082-191">Click **Submit**.</span></span>

> [!TIP]
> <span data-ttu-id="ad082-192">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="ad082-192">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ad082-193">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="ad082-193">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ad082-194">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ad082-194">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="ad082-195">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ad082-195">Create an Azure AD test user</span></span>
<span data-ttu-id="ad082-196">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ad082-196">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="ad082-198">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ad082-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ad082-199">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="ad082-199">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ad082-201">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="ad082-201">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ad082-203">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ad082-203">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ad082-205">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ad082-205">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ad082-207">a.</span><span class="sxs-lookup"><span data-stu-id="ad082-207">a.</span></span> <span data-ttu-id="ad082-208">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ad082-208">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ad082-209">b.</span><span class="sxs-lookup"><span data-stu-id="ad082-209">b.</span></span> <span data-ttu-id="ad082-210">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ad082-210">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ad082-211">c.</span><span class="sxs-lookup"><span data-stu-id="ad082-211">c.</span></span> <span data-ttu-id="ad082-212">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="ad082-212">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ad082-213">d.</span><span class="sxs-lookup"><span data-stu-id="ad082-213">d.</span></span> <span data-ttu-id="ad082-214">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ad082-214">Click **Create**.</span></span>
 
### <a name="create-a-rfpio-test-user"></a><span data-ttu-id="ad082-215">Vytvoření zkušebního uživatele RFPIO</span><span class="sxs-lookup"><span data-stu-id="ad082-215">Create a RFPIO test user</span></span>

<span data-ttu-id="ad082-216">Pokud chcete povolit uživatelům Azure AD přihlášení k RFPIO, musí být zřízená do RFPIO.</span><span class="sxs-lookup"><span data-stu-id="ad082-216">To enable Azure AD users to log in to RFPIO, they must be provisioned into RFPIO.</span></span>  
<span data-ttu-id="ad082-217">V případě RFPIO zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="ad082-217">In the case of RFPIO, provisioning is a manual task.</span></span>

<span data-ttu-id="ad082-218">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ad082-218">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="ad082-219">Přihlaste se k serveru vaší společnosti RFPIO jako správce.</span><span class="sxs-lookup"><span data-stu-id="ad082-219">Log in to your RFPIO company site as an administrator.</span></span>

2. <span data-ttu-id="ad082-220">Klikněte na rozevírací nabídce dolního rohu.</span><span class="sxs-lookup"><span data-stu-id="ad082-220">Click on the bottom left corner dropdown.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app1.png)

3. <span data-ttu-id="ad082-222">Klikněte na **nastavení organizace**.</span><span class="sxs-lookup"><span data-stu-id="ad082-222">Click on the **Organization Settings**.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app2.png)

4. <span data-ttu-id="ad082-224">Klikněte na tlačítko **členové týmu**.</span><span class="sxs-lookup"><span data-stu-id="ad082-224">Click **TEAM MEMBERS**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app6.png)

5. <span data-ttu-id="ad082-226">Klikněte na **přidat členy**.</span><span class="sxs-lookup"><span data-stu-id="ad082-226">Click on **ADD MEMBERS**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app7.png)

6. <span data-ttu-id="ad082-228">V **přidat nové členy** části.</span><span class="sxs-lookup"><span data-stu-id="ad082-228">In the **Add New Members** section.</span></span> <span data-ttu-id="ad082-229">Proveďte následující akce:</span><span class="sxs-lookup"><span data-stu-id="ad082-229">Perform following actions:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/app8.png)

    <span data-ttu-id="ad082-231">a.</span><span class="sxs-lookup"><span data-stu-id="ad082-231">a.</span></span> <span data-ttu-id="ad082-232">Zadejte **e-mailová adresa** v **zadejte jednu e-mailovou na každý řádek** pole.</span><span class="sxs-lookup"><span data-stu-id="ad082-232">Enter **Email address** in the **Enter one email per line** field.</span></span>

    <span data-ttu-id="ad082-233">b.</span><span class="sxs-lookup"><span data-stu-id="ad082-233">b.</span></span> <span data-ttu-id="ad082-234">Vyberte Plese **Role** podle požadavků.</span><span class="sxs-lookup"><span data-stu-id="ad082-234">Plese select **Role** according your requirements.</span></span>

    <span data-ttu-id="ad082-235">c.</span><span class="sxs-lookup"><span data-stu-id="ad082-235">c.</span></span> <span data-ttu-id="ad082-236">Klikněte na tlačítko **přidat členy**.</span><span class="sxs-lookup"><span data-stu-id="ad082-236">Click **ADD MEMBERS**.</span></span>
        
    > [!NOTE]
    > <span data-ttu-id="ad082-237">Držitel účtu Azure Active Directory obdrží e-mailu a dodržuje odkaz potvrďte svůj účet, pak se změní na aktivní.</span><span class="sxs-lookup"><span data-stu-id="ad082-237">The Azure Active Directory account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="ad082-238">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="ad082-238">Assign the Azure AD test user</span></span>

<span data-ttu-id="ad082-239">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu RFPIO.</span><span class="sxs-lookup"><span data-stu-id="ad082-239">In this section, you enable Britta Simon to use Azure single sign-on by granting access to RFPIO.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="ad082-241">**Pokud chcete přiřadit Britta Simon RFPIO, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="ad082-241">**To assign Britta Simon to RFPIO, perform the following steps:**</span></span>

1. <span data-ttu-id="ad082-242">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ad082-242">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="ad082-244">V seznamu aplikací vyberte **RFPIO**.</span><span class="sxs-lookup"><span data-stu-id="ad082-244">In the applications list, select **RFPIO**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_app.png) 

3. <span data-ttu-id="ad082-246">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="ad082-246">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="ad082-248">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ad082-248">Click **Add** button.</span></span> <span data-ttu-id="ad082-249">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ad082-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="ad082-251">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="ad082-251">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ad082-252">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ad082-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ad082-253">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ad082-253">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="ad082-254">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="ad082-254">Test single sign-on</span></span>

<span data-ttu-id="ad082-255">V této části otestovat vaše konfigurace Azure AD jeden přihlašování pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="ad082-255">In this section, you test your Azure AD single sign-on configuration by using the Access Panel.</span></span>

<span data-ttu-id="ad082-256">Když kliknete na dlaždici RFPIO na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci RFPIO.</span><span class="sxs-lookup"><span data-stu-id="ad082-256">When you click the RFPIO tile in the Access Panel, you should get automatically signed-on to your RFPIO application.</span></span>
<span data-ttu-id="ad082-257">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ad082-257">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ad082-258">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="ad082-258">Additional resources</span></span>

* [<span data-ttu-id="ad082-259">Seznam kurzů o tom, jak integraci aplikací SaaS v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ad082-259">List of tutorials about how to integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ad082-260">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ad082-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_203.png
