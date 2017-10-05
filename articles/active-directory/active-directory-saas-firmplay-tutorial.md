---
title: "Kurz: Azure Active Directory integrace s FirmPlay - hájící zájmy zaměstnanec pro nábor | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a FirmPlay - hájící zájmy zaměstnanec pro nábor."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a6799629-7546-43f8-a966-956db32864b1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: jeedes
ms.openlocfilehash: 3cddd5b9508159089bf344dbb3882d462799747c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-firmplay---employee-advocacy-for-recruiting"></a><span data-ttu-id="59275-103">Kurz: Azure Active Directory integrace s FirmPlay - hájící zájmy zaměstnanec pro nábor</span><span class="sxs-lookup"><span data-stu-id="59275-103">Tutorial: Azure Active Directory integration with FirmPlay - Employee Advocacy for Recruiting</span></span>

<span data-ttu-id="59275-104">V tomto kurzu zjistěte, jak integrovat FirmPlay - hájící zájmy zaměstnanec pro nábor službou Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="59275-104">In this tutorial, you learn how to integrate FirmPlay - Employee Advocacy for Recruiting with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="59275-105">Integrace FirmPlay - hájící zájmy zaměstnanec pro nábor s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="59275-105">Integrating FirmPlay - Employee Advocacy for Recruiting with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="59275-106">Můžete řídit ve službě Azure AD, který má přístup k FirmPlay - hájící zájmy zaměstnanec pro nábor</span><span class="sxs-lookup"><span data-stu-id="59275-106">You can control in Azure AD who has access to FirmPlay - Employee Advocacy for Recruiting</span></span>
- <span data-ttu-id="59275-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k FirmPlay - hájící zájmy zaměstnanec pro nábor (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="59275-107">You can enable your users to automatically get signed-on to FirmPlay - Employee Advocacy for Recruiting (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="59275-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="59275-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="59275-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="59275-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="59275-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="59275-110">Prerequisites</span></span>

<span data-ttu-id="59275-111">Konfigurace integrace Azure AD s FirmPlay - hájící zájmy zaměstnanec pro nábor, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="59275-111">To configure Azure AD integration with FirmPlay - Employee Advocacy for Recruiting, you need the following items:</span></span>

- <span data-ttu-id="59275-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="59275-112">An Azure AD subscription</span></span>
- <span data-ttu-id="59275-113">FirmPlay - hájící zájmy zaměstnanec pro jednotné přihlašování v předplatném povolené o přijetí</span><span class="sxs-lookup"><span data-stu-id="59275-113">A FirmPlay - Employee Advocacy for Recruiting single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="59275-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="59275-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="59275-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="59275-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="59275-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="59275-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="59275-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="59275-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="59275-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="59275-118">Scenario description</span></span>
<span data-ttu-id="59275-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="59275-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="59275-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="59275-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="59275-121">Přidání FirmPlay - hájící zájmy zaměstnanec pro nábor z Galerie</span><span class="sxs-lookup"><span data-stu-id="59275-121">Adding FirmPlay - Employee Advocacy for Recruiting from the gallery</span></span>
2. <span data-ttu-id="59275-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="59275-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-firmplay---employee-advocacy-for-recruiting-from-the-gallery"></a><span data-ttu-id="59275-123">Přidání FirmPlay - hájící zájmy zaměstnanec pro nábor z Galerie</span><span class="sxs-lookup"><span data-stu-id="59275-123">Adding FirmPlay - Employee Advocacy for Recruiting from the gallery</span></span>
<span data-ttu-id="59275-124">Konfigurace integrace FirmPlay - hájící zájmy zaměstnanec pro nábor do služby Azure AD, potřebujete přidat FirmPlay - hájící zájmy zaměstnanec pro nábor z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="59275-124">To configure the integration of FirmPlay - Employee Advocacy for Recruiting into Azure AD, you need to add FirmPlay - Employee Advocacy for Recruiting from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="59275-125">**Chcete-li přidat FirmPlay - hájící zájmy zaměstnanec pro nábor z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="59275-125">**To add FirmPlay - Employee Advocacy for Recruiting from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="59275-126">V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="59275-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="59275-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="59275-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="59275-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="59275-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="59275-131">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="59275-131">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="59275-133">Do vyhledávacího pole zadejte **FirmPlay - hájící zájmy zaměstnanec pro nábor**.</span><span class="sxs-lookup"><span data-stu-id="59275-133">In the search box, type **FirmPlay - Employee Advocacy for Recruiting**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_001.png)

5. <span data-ttu-id="59275-135">Na panelu výsledků vyberte **FirmPlay - hájící zájmy zaměstnanec pro nábor**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="59275-135">In the results panel, select **FirmPlay - Employee Advocacy for Recruiting**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="59275-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="59275-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="59275-138">V této části konfiguraci a testování Azure AD jednotné přihlašování s FirmPlay - hájící zájmy zaměstnanec pro nábor podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="59275-138">In this section, you configure and test Azure AD single sign-on with FirmPlay - Employee Advocacy for Recruiting based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="59275-139">Pro jednotné přihlašování pro práci Azure AD je potřeba vědět, jaké příslušného uživatele v FirmPlay – hájící zájmy zaměstnanec pro nábor je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="59275-139">For single sign-on to work, Azure AD needs to know what the counterpart user in FirmPlay - Employee Advocacy for Recruiting is to a user in Azure AD.</span></span> <span data-ttu-id="59275-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v FirmPlay - hájící zájmy zaměstnanec pro přijímání musí být vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="59275-140">In other words, a link relationship between an Azure AD user and the related user in FirmPlay - Employee Advocacy for Recruiting needs to be established.</span></span>

<span data-ttu-id="59275-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v FirmPlay - hájící zájmy zaměstnanec pro nábor.</span><span class="sxs-lookup"><span data-stu-id="59275-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in FirmPlay - Employee Advocacy for Recruiting.</span></span>

<span data-ttu-id="59275-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s FirmPlay - hájící zájmy zaměstnanec pro nábor, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="59275-142">To configure and test Azure AD single sign-on with FirmPlay - Employee Advocacy for Recruiting, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="59275-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="59275-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="59275-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="59275-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="59275-145">**[Vytváření FirmPlay - hájící zájmy zaměstnanec pro nábor testovacího uživatele](#creating-a-firmplay---employee-advocacy-for-recruiting-test-user)**  – Pokud chcete mít protějšek Britta Simon v FirmPlay: hájící zájmy zaměstnanců, pro který je o přijetí propojené s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="59275-145">**[Creating a FirmPlay - Employee Advocacy for Recruiting test user](#creating-a-firmplay---employee-advocacy-for-recruiting-test-user)** - to have a counterpart of Britta Simon in FirmPlay: Employee Advocacy for Recruiting that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="59275-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="59275-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="59275-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="59275-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="59275-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="59275-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="59275-149">V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v vaší FirmPlay - hájící zájmy zaměstnanec nábor aplikace.</span><span class="sxs-lookup"><span data-stu-id="59275-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your FirmPlay - Employee Advocacy for Recruiting application.</span></span>

<span data-ttu-id="59275-150">**Ke konfiguraci Azure AD jednotné přihlašování s FirmPlay - hájící zájmy zaměstnanec pro nábor, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="59275-150">**To configure Azure AD single sign-on with FirmPlay - Employee Advocacy for Recruiting, perform the following steps:**</span></span>

1. <span data-ttu-id="59275-151">Na portálu Azure Management portal na **FirmPlay - hájící zájmy zaměstnanec pro nábor** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="59275-151">In the Azure Management portal, on the **FirmPlay - Employee Advocacy for Recruiting** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="59275-153">Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="59275-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_01.png)

3. <span data-ttu-id="59275-155">Na **FirmPlay - hájící zájmy zaměstnanec domény o přijetí a adresy URL** v části **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<your-subdomain>.firmplay.com/`</span><span class="sxs-lookup"><span data-stu-id="59275-155">On the **FirmPlay - Employee Advocacy for Recruiting Domain and URLs** section, in the **Sign On URL** textbox, type a URL using the following pattern: `https://<your-subdomain>.firmplay.com/`</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_02.png)

    > [!NOTE] 
    > <span data-ttu-id="59275-157">Upozorňujeme, že se nejedná skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="59275-157">Please note that this is not the real value.</span></span> <span data-ttu-id="59275-158">Budete muset aktualizovat tuto hodnotu s skutečné přihlašovací na adresy URL.</span><span class="sxs-lookup"><span data-stu-id="59275-158">You have to update this value with the actual Sign On URL.</span></span> <span data-ttu-id="59275-159">Obraťte se na [FirmPlay - hájící zájmy zaměstnanec pro tým podpory nábor](mailto:engineering@firmplay.com) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="59275-159">Contact [FirmPlay - Employee Advocacy for Recruiting support team](mailto:engineering@firmplay.com) to get this value.</span></span> 

4. <span data-ttu-id="59275-160">Na **SAML podpisový certifikát** klikněte na tlačítko **vytvořit nový certifikát**.</span><span class="sxs-lookup"><span data-stu-id="59275-160">On the **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_03.png)   

5. <span data-ttu-id="59275-162">Na **vytvořit nový certifikát** dialogové okno, klikněte na ikonu kalendáři a vyberte **datum vypršení platnosti**.</span><span class="sxs-lookup"><span data-stu-id="59275-162">On the **Create New Certificate** dialog, click the calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="59275-163">Pak klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="59275-163">Then click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="59275-165">Na **SAML podpisový certifikát** vyberte **aktivujte nový certifikát** a klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="59275-165">On the **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_04.png)

7. <span data-ttu-id="59275-167">V místní nabídce **certifikát výměny** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="59275-167">On the pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="59275-169">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="59275-169">On the **SAML Signing Certificate** section, click **Certificate (base64)** and then save the certificate file on your computer.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_05.png) 

9. <span data-ttu-id="59275-171">Na **FirmPlay - hájící zájmy zaměstnanec pro přijetí konfigurace** klikněte na tlačítko **konfigurace FirmPlay - hájící zájmy zaměstnanec pro nábor** otevřete **konfigurovat přihlášení** Dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="59275-171">On the **FirmPlay - Employee Advocacy for Recruiting Configuration** section, click **Configure FirmPlay - Employee Advocacy for Recruiting** to open **Configure sign-on** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_06.png) 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_07.png)

10. <span data-ttu-id="59275-174">Chcete-li získat jednotné přihlašování, které jsou nakonfigurované pro vaše aplikace, obraťte se na [FirmPlay - hájící zájmy zaměstnanec pro tým podpory nábor](mailto:engineering@firmplay.com) a poskytněte jim následující:</span><span class="sxs-lookup"><span data-stu-id="59275-174">To get SSO configured for your application, contact [FirmPlay - Employee Advocacy for Recruiting support team](mailto:engineering@firmplay.com) and provide them with the following:</span></span> 

    <span data-ttu-id="59275-175">• Stažené **soubor certifikátu**</span><span class="sxs-lookup"><span data-stu-id="59275-175">•  The downloaded **Certificate file**</span></span>

    <span data-ttu-id="59275-176">• **Adresa URL služby jednotného přihlašování SAML**</span><span class="sxs-lookup"><span data-stu-id="59275-176">•  The **SAML Single Sign-On Service URL**</span></span>

    <span data-ttu-id="59275-177">• **SAML Entity ID**</span><span class="sxs-lookup"><span data-stu-id="59275-177">•  The **SAML Entity ID**</span></span>

    <span data-ttu-id="59275-178">• **Odhlášení adresy URL**</span><span class="sxs-lookup"><span data-stu-id="59275-178">•  The **Sign-Out URL**</span></span>
  

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="59275-179">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="59275-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="59275-180">Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="59275-180">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="59275-182">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="59275-182">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="59275-183">V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="59275-183">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-firmplay-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="59275-185">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="59275-185">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-firmplay-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="59275-187">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="59275-187">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-firmplay-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="59275-189">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="59275-189">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-firmplay-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="59275-191">a.</span><span class="sxs-lookup"><span data-stu-id="59275-191">a.</span></span> <span data-ttu-id="59275-192">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="59275-192">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="59275-193">b.</span><span class="sxs-lookup"><span data-stu-id="59275-193">b.</span></span> <span data-ttu-id="59275-194">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="59275-194">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="59275-195">c.</span><span class="sxs-lookup"><span data-stu-id="59275-195">c.</span></span> <span data-ttu-id="59275-196">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="59275-196">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="59275-197">d.</span><span class="sxs-lookup"><span data-stu-id="59275-197">d.</span></span> <span data-ttu-id="59275-198">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="59275-198">Click **Create**.</span></span> 



### <a name="creating-a-firmplay---employee-advocacy-for-recruiting-test-user"></a><span data-ttu-id="59275-199">Vytváření FirmPlay - hájící zájmy zaměstnanec pro nábor testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="59275-199">Creating a FirmPlay - Employee Advocacy for Recruiting test user</span></span>

<span data-ttu-id="59275-200">V této části vytvoříte volal Britta Simon v FirmPlay - hájící zájmy zaměstnanec pro nábor uživatele.</span><span class="sxs-lookup"><span data-stu-id="59275-200">In this section, you create a user called Britta Simon in FirmPlay - Employee Advocacy for Recruiting.</span></span> <span data-ttu-id="59275-201">Spojte se s [FirmPlay - hájící zájmy zaměstnanec pro tým podpory nábor](mailto:engineering@firmplay.com) přidat uživatele do FirmPlay - hájící zájmy zaměstnanec pro nábor platformu.</span><span class="sxs-lookup"><span data-stu-id="59275-201">Please work with [FirmPlay - Employee Advocacy for Recruiting support team](mailto:engineering@firmplay.com) to add the users in the FirmPlay - Employee Advocacy for Recruiting platform.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="59275-202">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="59275-202">Assigning the Azure AD test user</span></span>

<span data-ttu-id="59275-203">V této části povolíte Britta Simon používat tak, že udělíte přístup k FirmPlay - hájící zájmy zaměstnanec pro nábor Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="59275-203">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to FirmPlay - Employee Advocacy for Recruiting.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="59275-205">**Britta Simon přiřadit FirmPlay - hájící zájmy zaměstnanec pro nábor, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="59275-205">**To assign Britta Simon to FirmPlay - Employee Advocacy for Recruiting, perform the following steps:**</span></span>

1. <span data-ttu-id="59275-206">V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="59275-206">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="59275-208">V seznamu aplikací vyberte **FirmPlay - hájící zájmy zaměstnanec pro nábor**.</span><span class="sxs-lookup"><span data-stu-id="59275-208">In the applications list, select **FirmPlay - Employee Advocacy for Recruiting**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-firmplay-tutorial/tutorial_firmplay_50.png) 

3. <span data-ttu-id="59275-210">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="59275-210">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="59275-212">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="59275-212">Click **Add** button.</span></span> <span data-ttu-id="59275-213">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="59275-213">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="59275-215">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="59275-215">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="59275-216">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="59275-216">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="59275-217">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="59275-217">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="59275-218">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="59275-218">Testing single sign-on</span></span>

<span data-ttu-id="59275-219">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="59275-219">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="59275-220">Po kliknutí na tlačítko FirmPlay - hájící zájmy zaměstnanec pro dlaždici nábor na přístupovém panelu jste měli získat automaticky přihlášení k vaší FirmPlay - hájící zájmy zaměstnanec nábor aplikace.</span><span class="sxs-lookup"><span data-stu-id="59275-220">When you click the FirmPlay - Employee Advocacy for Recruiting tile in the Access Panel, you should get automatically signed-on to your FirmPlay - Employee Advocacy for Recruiting application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="59275-221">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="59275-221">Additional resources</span></span>

* [<span data-ttu-id="59275-222">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="59275-222">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="59275-223">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="59275-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-firmplay-tutorial/tutorial_general_203.png