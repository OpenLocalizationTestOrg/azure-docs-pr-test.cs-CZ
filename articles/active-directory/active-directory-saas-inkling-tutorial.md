---
title: 'Kurz: Azure Active Directory integrace s Inkling | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Inkling."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 64c7ee45-ee8a-42f7-bf04-fd0e00833ea9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.openlocfilehash: 7b0639c6515298731f88346c2e4ca82664653a2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-inkling"></a><span data-ttu-id="7354a-103">Kurz: Azure Active Directory integrace s Inkling</span><span class="sxs-lookup"><span data-stu-id="7354a-103">Tutorial: Azure Active Directory integration with Inkling</span></span>

<span data-ttu-id="7354a-104">V tomto kurzu zjistěte, jak integrovat Inkling s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7354a-104">In this tutorial, you learn how to integrate Inkling with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7354a-105">Integrace Inkling s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="7354a-105">Integrating Inkling with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7354a-106">Můžete řídit ve službě Azure AD, který má přístup k Inkling</span><span class="sxs-lookup"><span data-stu-id="7354a-106">You can control in Azure AD who has access to Inkling</span></span>
- <span data-ttu-id="7354a-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Inkling (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="7354a-107">You can enable your users to automatically get signed-on to Inkling (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7354a-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="7354a-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="7354a-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7354a-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7354a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7354a-110">Prerequisites</span></span>

<span data-ttu-id="7354a-111">Konfigurace integrace Azure AD s Inkling, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="7354a-111">To configure Azure AD integration with Inkling, you need the following items:</span></span>

- <span data-ttu-id="7354a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="7354a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7354a-113">Inkling jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="7354a-113">An Inkling single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="7354a-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="7354a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="7354a-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="7354a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7354a-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="7354a-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="7354a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7354a-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="7354a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="7354a-118">Scenario description</span></span>
<span data-ttu-id="7354a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="7354a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7354a-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="7354a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7354a-121">Přidání Inkling z Galerie</span><span class="sxs-lookup"><span data-stu-id="7354a-121">Adding Inkling from the gallery</span></span>
2. <span data-ttu-id="7354a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7354a-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-inkling-from-the-gallery"></a><span data-ttu-id="7354a-123">Přidání Inkling z Galerie</span><span class="sxs-lookup"><span data-stu-id="7354a-123">Adding Inkling from the gallery</span></span>
<span data-ttu-id="7354a-124">Při konfiguraci integrace Inkling do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Inkling z galerie.</span><span class="sxs-lookup"><span data-stu-id="7354a-124">To configure the integration of Inkling into Azure AD, you need to add Inkling from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7354a-125">**Pokud chcete přidat Inkling z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7354a-125">**To add Inkling from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7354a-126">V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7354a-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7354a-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="7354a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7354a-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7354a-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="7354a-131">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7354a-131">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="7354a-133">Do vyhledávacího pole zadejte **Inkling**.</span><span class="sxs-lookup"><span data-stu-id="7354a-133">In the search box, type **Inkling**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_001.png)

5. <span data-ttu-id="7354a-135">Na panelu výsledků vyberte **Inkling**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7354a-135">In the results panel, select **Inkling**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7354a-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7354a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7354a-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Inkling podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="7354a-138">In this section, you configure and test Azure AD single sign-on with Inkling based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7354a-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Inkling je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7354a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Inkling is to a user in Azure AD.</span></span> <span data-ttu-id="7354a-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Inkling musí navázat.</span><span class="sxs-lookup"><span data-stu-id="7354a-140">In other words, a link relationship between an Azure AD user and the related user in Inkling needs to be established.</span></span>

<span data-ttu-id="7354a-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Inkling.</span><span class="sxs-lookup"><span data-stu-id="7354a-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Inkling.</span></span>

<span data-ttu-id="7354a-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Inkling, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="7354a-142">To configure and test Azure AD single sign-on with Inkling, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7354a-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="7354a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7354a-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7354a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7354a-145">**[Vytváření testovacího uživatele Inkling](#creating-an-inkling-test-user)**  – Pokud chcete mít protějšek Britta Simon v Inkling propojeném s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="7354a-145">**[Creating an Inkling test user](#creating-an-inkling-test-user)** - to have a counterpart of Britta Simon in Inkling that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="7354a-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7354a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7354a-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="7354a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7354a-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7354a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7354a-149">V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v aplikaci Inkling.</span><span class="sxs-lookup"><span data-stu-id="7354a-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Inkling application.</span></span>

<span data-ttu-id="7354a-150">**Ke konfiguraci Azure AD jednotné přihlašování s Inkling, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7354a-150">**To configure Azure AD single sign-on with Inkling, perform the following steps:**</span></span>

1. <span data-ttu-id="7354a-151">Na portálu Azure Management portal na **Inkling** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="7354a-151">In the Azure Management portal, on the **Inkling** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="7354a-153">Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="7354a-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-inkling-tutorial/tutorial_general_300.png)
    
3. <span data-ttu-id="7354a-155">Na **Inkling domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7354a-155">On the **Inkling Domain and URLs** section, perform the following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_01.png)

    <span data-ttu-id="7354a-157">a.</span><span class="sxs-lookup"><span data-stu-id="7354a-157">a.</span></span> <span data-ttu-id="7354a-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://api.inkling.com/saml/v2/metadata/<user-id>`</span><span class="sxs-lookup"><span data-stu-id="7354a-158">In the **Identifier** textbox, type a URL using the following pattern: `https://api.inkling.com/saml/v2/metadata/<user-id>`</span></span>

    <span data-ttu-id="7354a-159">b.</span><span class="sxs-lookup"><span data-stu-id="7354a-159">b.</span></span> <span data-ttu-id="7354a-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://api.inkling.com/saml/v2/acs/<user-id>`</span><span class="sxs-lookup"><span data-stu-id="7354a-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://api.inkling.com/saml/v2/acs/<user-id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7354a-161">Upozorňujeme, že tyto nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7354a-161">Please note that these are not the real values.</span></span> <span data-ttu-id="7354a-162">Budete muset aktualizovat tyto hodnoty se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7354a-162">You have to update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="7354a-163">Obraťte se na [tým podpory Inkling](mailto:press@inkling.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="7354a-163">Contact [Inkling support team](mailto:press@inkling.com) to get these values.</span></span>

4. <span data-ttu-id="7354a-164">Na **SAML podpisový certifikát** klikněte na tlačítko **vytvořit nový certifikát**.</span><span class="sxs-lookup"><span data-stu-id="7354a-164">On the **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-inkling-tutorial/tutorial_general_400.png)    

5. <span data-ttu-id="7354a-166">Na **vytvořit nový certifikát** dialogové okno, klikněte na ikonu kalendáři a vyberte **datum vypršení platnosti**.</span><span class="sxs-lookup"><span data-stu-id="7354a-166">On the **Create New Certificate** dialog, click the calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="7354a-167">Pak klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7354a-167">Then click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-inkling-tutorial/tutorial_general_500.png)

6. <span data-ttu-id="7354a-169">Na **SAML podpisový certifikát** vyberte **aktivujte nový certifikát** a klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7354a-169">On the **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_02.png)

7. <span data-ttu-id="7354a-171">V místní nabídce **certifikát výměny** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="7354a-171">On the pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-inkling-tutorial/tutorial_general_600.png)

8. <span data-ttu-id="7354a-173">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="7354a-173">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_03.png) 

9. <span data-ttu-id="7354a-175">Pokud chcete získat jednotné přihlašování, které jsou nakonfigurované pro vaše aplikace, obraťte se na [tým podpory Inkling](mailto:press@inkling.com) a poskytněte s Stáhnout **metadata**.</span><span class="sxs-lookup"><span data-stu-id="7354a-175">To get SSO configured for your application, contact [Inkling support team](mailto:press@inkling.com) and provide them with downloaded **metadata**.</span></span> 


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7354a-176">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="7354a-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="7354a-177">Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7354a-177">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="7354a-179">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7354a-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7354a-180">V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7354a-180">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-inkling-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7354a-182">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="7354a-182">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-inkling-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7354a-184">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7354a-184">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-inkling-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7354a-186">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7354a-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-inkling-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7354a-188">a.</span><span class="sxs-lookup"><span data-stu-id="7354a-188">a.</span></span> <span data-ttu-id="7354a-189">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7354a-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7354a-190">b.</span><span class="sxs-lookup"><span data-stu-id="7354a-190">b.</span></span> <span data-ttu-id="7354a-191">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7354a-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7354a-192">c.</span><span class="sxs-lookup"><span data-stu-id="7354a-192">c.</span></span> <span data-ttu-id="7354a-193">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="7354a-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="7354a-194">d.</span><span class="sxs-lookup"><span data-stu-id="7354a-194">d.</span></span> <span data-ttu-id="7354a-195">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7354a-195">Click **Create**.</span></span> 



### <a name="creating-an-inkling-test-user"></a><span data-ttu-id="7354a-196">Vytváření testovacího uživatele Inkling</span><span class="sxs-lookup"><span data-stu-id="7354a-196">Creating an Inkling test user</span></span>

<span data-ttu-id="7354a-197">V této části vytvoříte volal Britta Simon v Inkling uživatele.</span><span class="sxs-lookup"><span data-stu-id="7354a-197">In this section, you create a user called Britta Simon in Inkling.</span></span> <span data-ttu-id="7354a-198">Spojte se s [tým podpory Inkling](mailto:press@inkling.com) přidat uživatele do Inkling platformy.</span><span class="sxs-lookup"><span data-stu-id="7354a-198">Please work with [Inkling support team](mailto:press@inkling.com) to add the users in the Inkling platform.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="7354a-199">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="7354a-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="7354a-200">V této části povolíte Britta Simon používat tak, že udělíte přístup k Inkling Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7354a-200">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Inkling.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="7354a-202">**Pokud chcete přiřadit Britta Simon Inkling, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="7354a-202">**To assign Britta Simon to Inkling, perform the following steps:**</span></span>

1. <span data-ttu-id="7354a-203">V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7354a-203">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="7354a-205">V seznamu aplikací vyberte **Inkling**.</span><span class="sxs-lookup"><span data-stu-id="7354a-205">In the applications list, select **Inkling**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_50.png) 

3. <span data-ttu-id="7354a-207">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="7354a-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="7354a-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7354a-209">Click **Add** button.</span></span> <span data-ttu-id="7354a-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7354a-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="7354a-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="7354a-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7354a-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7354a-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7354a-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7354a-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="7354a-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7354a-215">Testing single sign-on</span></span>

<span data-ttu-id="7354a-216">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="7354a-216">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="7354a-217">Když kliknete na dlaždici Inkling na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Inkling.</span><span class="sxs-lookup"><span data-stu-id="7354a-217">When you click the Inkling tile in the Access Panel, you should get automatically signed-on to your Inkling application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="7354a-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7354a-218">Additional resources</span></span>

* [<span data-ttu-id="7354a-219">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7354a-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7354a-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7354a-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_203.png