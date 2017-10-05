---
title: 'Kurz: Azure Active Directory integrace s Fuze | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Fuze."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9780b4bf-1fd1-48c1-9ceb-f750225ae08a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeedes
ms.openlocfilehash: c7f7b095aac6202a7ec5248ee2bbb109615287a7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-fuze"></a><span data-ttu-id="b303a-103">Kurz: Azure Active Directory integrace s Fuze</span><span class="sxs-lookup"><span data-stu-id="b303a-103">Tutorial: Azure Active Directory integration with Fuze</span></span>

<span data-ttu-id="b303a-104">V tomto kurzu zjistěte, jak integrovat Fuze s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b303a-104">In this tutorial, you learn how to integrate Fuze with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b303a-105">Integrace Fuze s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b303a-105">Integrating Fuze with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b303a-106">Můžete řídit ve službě Azure AD, který má přístup k Fuze</span><span class="sxs-lookup"><span data-stu-id="b303a-106">You can control in Azure AD who has access to Fuze</span></span>
- <span data-ttu-id="b303a-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Fuze (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b303a-107">You can enable your users to automatically get signed-on to Fuze (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b303a-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="b303a-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="b303a-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b303a-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b303a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b303a-110">Prerequisites</span></span>

<span data-ttu-id="b303a-111">Konfigurace integrace Azure AD s Fuze, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="b303a-111">To configure Azure AD integration with Fuze, you need the following items:</span></span>

- <span data-ttu-id="b303a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b303a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b303a-113">Fuze jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b303a-113">A Fuze single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="b303a-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b303a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="b303a-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b303a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b303a-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="b303a-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="b303a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b303a-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="b303a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b303a-118">Scenario description</span></span>
<span data-ttu-id="b303a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b303a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b303a-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b303a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b303a-121">Přidání Fuze z Galerie</span><span class="sxs-lookup"><span data-stu-id="b303a-121">Adding Fuze from the gallery</span></span>
2. <span data-ttu-id="b303a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b303a-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-fuze-from-the-gallery"></a><span data-ttu-id="b303a-123">Přidání Fuze z Galerie</span><span class="sxs-lookup"><span data-stu-id="b303a-123">Adding Fuze from the gallery</span></span>
<span data-ttu-id="b303a-124">Při konfiguraci integrace Fuze do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Fuze z galerie.</span><span class="sxs-lookup"><span data-stu-id="b303a-124">To configure the integration of Fuze into Azure AD, you need to add Fuze from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b303a-125">**Pokud chcete přidat Fuze z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b303a-125">**To add Fuze from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b303a-126">V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b303a-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b303a-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b303a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b303a-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b303a-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b303a-131">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b303a-131">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b303a-133">Do vyhledávacího pole zadejte **Fuze**.</span><span class="sxs-lookup"><span data-stu-id="b303a-133">In the search box, type **Fuze**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_000.png)

5. <span data-ttu-id="b303a-135">Na panelu výsledků vyberte **Fuze**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b303a-135">In the results panel, select **Fuze**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b303a-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b303a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b303a-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Fuze podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="b303a-138">In this section, you configure and test Azure AD single sign-on with Fuze based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b303a-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Fuze je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b303a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Fuze is to a user in Azure AD.</span></span> <span data-ttu-id="b303a-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Fuze musí navázat.</span><span class="sxs-lookup"><span data-stu-id="b303a-140">In other words, a link relationship between an Azure AD user and the related user in Fuze needs to be established.</span></span>

<span data-ttu-id="b303a-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Fuze.</span><span class="sxs-lookup"><span data-stu-id="b303a-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Fuze.</span></span>

<span data-ttu-id="b303a-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Fuze, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="b303a-142">To configure and test Azure AD single sign-on with Fuze, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b303a-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="b303a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b303a-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b303a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b303a-145">**[Vytvoření zkušebního uživatele Fuze](#creating-a-fuze-test-user)**  – Pokud chcete mít protějšek Britta Simon v Fuze propojeném s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="b303a-145">**[Creating a Fuze test user](#creating-a-fuze-test-user)** - to have a counterpart of Britta Simon in Fuze that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="b303a-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b303a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b303a-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="b303a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b303a-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b303a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b303a-149">V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v aplikaci Fuze.</span><span class="sxs-lookup"><span data-stu-id="b303a-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Fuze application.</span></span>

<span data-ttu-id="b303a-150">**Ke konfiguraci Azure AD jednotné přihlašování s Fuze, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b303a-150">**To configure Azure AD single sign-on with Fuze, perform the following steps:**</span></span>

1. <span data-ttu-id="b303a-151">Na portálu Azure Management portal na **Fuze** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b303a-151">In the Azure Management portal, on the **Fuze** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b303a-153">Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="b303a-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_01.png)

3. <span data-ttu-id="b303a-155">Na **Fuze domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b303a-155">On the **Fuze Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_020.png)
    
    <span data-ttu-id="b303a-157">V **přihlásit na adrese URL** textovému poli, typ přihlašování v adrese URL jako:`https://www.thinkingphones.com/jetspeed/portal/`</span><span class="sxs-lookup"><span data-stu-id="b303a-157">In the **Sign on URL** textbox, type the Sign-on URL as: `https://www.thinkingphones.com/jetspeed/portal/`</span></span>

4.  <span data-ttu-id="b303a-158">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b303a-158">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fuze-tutorial/tutorial_general_400.png)

5. <span data-ttu-id="b303a-160">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor xml ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b303a-160">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the xml file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_05.png) 

6. <span data-ttu-id="b303a-162">Konfigurace jednotného přihlašování na **Fuze** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory Fuze](https://www.fuze.com/support).</span><span class="sxs-lookup"><span data-stu-id="b303a-162">To configure single sign-on on **Fuze** side, you need to send the downloaded **Metadata XML** to [Fuze support team](https://www.fuze.com/support).</span></span> <span data-ttu-id="b303a-163">Toto se bude nastavení aby bylo možné používat jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="b303a-163">They will set this up in order to have the SAML SSO connection set properly on both sides.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b303a-164">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b303a-164">Creating an Azure AD test user</span></span>
<span data-ttu-id="b303a-165">Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b303a-165">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b303a-167">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b303a-167">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b303a-168">V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b303a-168">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fuze-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b303a-170">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b303a-170">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fuze-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b303a-172">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b303a-172">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fuze-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b303a-174">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b303a-174">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-fuze-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b303a-176">a.</span><span class="sxs-lookup"><span data-stu-id="b303a-176">a.</span></span> <span data-ttu-id="b303a-177">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b303a-177">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b303a-178">b.</span><span class="sxs-lookup"><span data-stu-id="b303a-178">b.</span></span> <span data-ttu-id="b303a-179">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b303a-179">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b303a-180">c.</span><span class="sxs-lookup"><span data-stu-id="b303a-180">c.</span></span> <span data-ttu-id="b303a-181">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b303a-181">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b303a-182">d.</span><span class="sxs-lookup"><span data-stu-id="b303a-182">d.</span></span> <span data-ttu-id="b303a-183">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b303a-183">Click **Create**.</span></span> 


### <a name="creating-a-fuze-test-user"></a><span data-ttu-id="b303a-184">Vytvoření zkušebního uživatele Fuze</span><span class="sxs-lookup"><span data-stu-id="b303a-184">Creating a Fuze test user</span></span>

<span data-ttu-id="b303a-185">Fuze aplikace podporuje úplné těsně v čas uživatele zřizovat, takže uživatelé budou automaticky vytvořeny při jejich přihlášení.</span><span class="sxs-lookup"><span data-stu-id="b303a-185">Fuze application supports full Just in time user provision, so users will get created automatically when they sign-in.</span></span> <span data-ttu-id="b303a-186">Další informace, kontaktujte prosím Fuze [podporu](https://www.fuze.com/support).</span><span class="sxs-lookup"><span data-stu-id="b303a-186">For any other clarification, please contact Fuze [support](https://www.fuze.com/support).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b303a-187">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b303a-187">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b303a-188">V této části povolíte Britta Simon používat tak, že udělíte přístup k Fuze Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b303a-188">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Fuze.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b303a-190">**Pokud chcete přiřadit Britta Simon Fuze, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="b303a-190">**To assign Britta Simon to Fuze, perform the following steps:**</span></span>

1. <span data-ttu-id="b303a-191">V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b303a-191">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b303a-193">V seznamu aplikací vyberte **Fuze**.</span><span class="sxs-lookup"><span data-stu-id="b303a-193">In the applications list, select **Fuze**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_50.png) 

3. <span data-ttu-id="b303a-195">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b303a-195">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b303a-197">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b303a-197">Click **Add** button.</span></span> <span data-ttu-id="b303a-198">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b303a-198">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b303a-200">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="b303a-200">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b303a-201">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b303a-201">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b303a-202">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b303a-202">Click **Assign** button on **Add Assignment** dialog.</span></span>
    

### <a name="testing-single-sign-on"></a><span data-ttu-id="b303a-203">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b303a-203">Testing single sign-on</span></span>

<span data-ttu-id="b303a-204">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b303a-204">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b303a-205">Když kliknete na dlaždici Fuze na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Fuze.</span><span class="sxs-lookup"><span data-stu-id="b303a-205">When you click the Fuze tile in the Access Panel, you should get automatically signed-on to your Fuze application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="b303a-206">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b303a-206">Additional resources</span></span>

* [<span data-ttu-id="b303a-207">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b303a-207">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b303a-208">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b303a-208">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_203.png