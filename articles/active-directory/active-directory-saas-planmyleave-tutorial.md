---
title: 'Kurz: Azure Active Directory integrace s PlanMyLeave | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a PlanMyLeave."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b0d31cbe-7ae2-488b-9cf3-4927391fa744
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: jeedes
ms.openlocfilehash: ba418a641b339a0d94a3c7b2596d37fbd88a30c5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-planmyleave"></a><span data-ttu-id="297af-103">Kurz: Azure Active Directory integrace s PlanMyLeave</span><span class="sxs-lookup"><span data-stu-id="297af-103">Tutorial: Azure Active Directory integration with PlanMyLeave</span></span>

<span data-ttu-id="297af-104">V tomto kurzu zjistěte, jak integrovat PlanMyLeave s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="297af-104">In this tutorial, you learn how to integrate PlanMyLeave with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="297af-105">Integrace PlanMyLeave s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="297af-105">Integrating PlanMyLeave with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="297af-106">Můžete řídit ve službě Azure AD, který má přístup k PlanMyLeave</span><span class="sxs-lookup"><span data-stu-id="297af-106">You can control in Azure AD who has access to PlanMyLeave</span></span>
- <span data-ttu-id="297af-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k PlanMyLeave (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="297af-107">You can enable your users to automatically get signed-on to PlanMyLeave (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="297af-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="297af-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="297af-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="297af-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="297af-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="297af-110">Prerequisites</span></span>

<span data-ttu-id="297af-111">Konfigurace integrace Azure AD s PlanMyLeave, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="297af-111">To configure Azure AD integration with PlanMyLeave, you need the following items:</span></span>

- <span data-ttu-id="297af-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="297af-112">An Azure AD subscription</span></span>
- <span data-ttu-id="297af-113">PlanMyLeave jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="297af-113">A PlanMyLeave single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="297af-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="297af-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="297af-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="297af-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="297af-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="297af-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="297af-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="297af-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="297af-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="297af-118">Scenario description</span></span>
<span data-ttu-id="297af-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="297af-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="297af-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="297af-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="297af-121">Přidání PlanMyLeave z Galerie</span><span class="sxs-lookup"><span data-stu-id="297af-121">Adding PlanMyLeave from the gallery</span></span>
2. <span data-ttu-id="297af-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="297af-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-planmyleave-from-the-gallery"></a><span data-ttu-id="297af-123">Přidání PlanMyLeave z Galerie</span><span class="sxs-lookup"><span data-stu-id="297af-123">Adding PlanMyLeave from the gallery</span></span>
<span data-ttu-id="297af-124">Při konfiguraci integrace PlanMyLeave do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS PlanMyLeave z galerie.</span><span class="sxs-lookup"><span data-stu-id="297af-124">To configure the integration of PlanMyLeave into Azure AD, you need to add PlanMyLeave from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="297af-125">**Pokud chcete přidat PlanMyLeave z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="297af-125">**To add PlanMyLeave from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="297af-126">V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="297af-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="297af-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="297af-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="297af-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="297af-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="297af-131">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="297af-131">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="297af-133">Do vyhledávacího pole zadejte **PlanMyLeave**.</span><span class="sxs-lookup"><span data-stu-id="297af-133">In the search box, type **PlanMyLeave**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_001.png)

5. <span data-ttu-id="297af-135">Na panelu výsledků vyberte **PlanMyLeave**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="297af-135">In the results panel, select **PlanMyLeave**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="297af-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="297af-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="297af-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s PlanMyLeave podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="297af-138">In this section, you configure and test Azure AD single sign-on with PlanMyLeave based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="297af-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v PlanMyLeave je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="297af-139">For single sign-on to work, Azure AD needs to know what the counterpart user in PlanMyLeave is to a user in Azure AD.</span></span> <span data-ttu-id="297af-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v PlanMyLeave musí navázat.</span><span class="sxs-lookup"><span data-stu-id="297af-140">In other words, a link relationship between an Azure AD user and the related user in PlanMyLeave needs to be established.</span></span>

<span data-ttu-id="297af-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v PlanMyLeave.</span><span class="sxs-lookup"><span data-stu-id="297af-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in PlanMyLeave.</span></span>

<span data-ttu-id="297af-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s PlanMyLeave, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="297af-142">To configure and test Azure AD single sign-on with PlanMyLeave, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="297af-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="297af-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="297af-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="297af-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="297af-145">**[Vytvoření zkušebního uživatele PlanMyLeave](#creating-a-planmyleave-test-user)**  – Pokud chcete mít protějšek Britta Simon v PlanMyLeave propojeném s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="297af-145">**[Creating a PlanMyLeave test user](#creating-a-planmyleave-test-user)** - to have a counterpart of Britta Simon in PlanMyLeave that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="297af-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="297af-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="297af-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="297af-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="297af-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="297af-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="297af-149">V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v aplikaci PlanMyLeave.</span><span class="sxs-lookup"><span data-stu-id="297af-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your PlanMyLeave application.</span></span>

<span data-ttu-id="297af-150">**Ke konfiguraci Azure AD jednotné přihlašování s PlanMyLeave, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="297af-150">**To configure Azure AD single sign-on with PlanMyLeave, perform the following steps:**</span></span>

1. <span data-ttu-id="297af-151">Na portálu Azure Management portal na **PlanMyLeave** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="297af-151">In the Azure Management portal, on the **PlanMyLeave** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="297af-153">Na **jednotného přihlašování** dialogové okno stránky, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="297af-153">On the **Single sign-on** dialog page, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_01.png)

3. <span data-ttu-id="297af-155">Na **PlanMyLeave domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="297af-155">On the **PlanMyLeave Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_02.png)

    <span data-ttu-id="297af-157">a.</span><span class="sxs-lookup"><span data-stu-id="297af-157">a.</span></span> <span data-ttu-id="297af-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company-name>.planmyleave.com/Login.aspx`</span><span class="sxs-lookup"><span data-stu-id="297af-158">In the **Sign On URL** textbox, type a URL using the following pattern: `https://<company-name>.planmyleave.com/Login.aspx`</span></span>
    
    <span data-ttu-id="297af-159">b.</span><span class="sxs-lookup"><span data-stu-id="297af-159">b.</span></span> <span data-ttu-id="297af-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company-name>.planmyleave.com`</span><span class="sxs-lookup"><span data-stu-id="297af-160">In the **Identifer** textbox, type a URL using the following pattern: `https://<company-name>.planmyleave.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="297af-161">Upozorňujeme, že tyto nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="297af-161">Please note that these are not the real values.</span></span> <span data-ttu-id="297af-162">Budete muset aktualizovat tyto hodnoty se skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="297af-162">You have to update these values with the actual Sign On URL and Identifier.</span></span> <span data-ttu-id="297af-163">Obraťte se na [tým podpory PlanMyLeave](mailto:support@planmyleave.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="297af-163">Contact [PlanMyLeave support team](mailto:support@planmyleave.com) to get these values.</span></span>

4. <span data-ttu-id="297af-164">Na **SAML podpisový certifikát** klikněte na tlačítko **vytvořit nový certifikát**.</span><span class="sxs-lookup"><span data-stu-id="297af-164">On the **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_03.png)     

5. <span data-ttu-id="297af-166">Na **vytvořit nový certifikát** dialogové okno, klikněte na ikonu kalendáři a vyberte **datum vypršení platnosti**.</span><span class="sxs-lookup"><span data-stu-id="297af-166">On the **Create New Certificate** dialog, click the calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="297af-167">Pak klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="297af-167">Then click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="297af-169">Na **SAML podpisový certifikát** vyberte **aktivujte nový certifikát** a klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="297af-169">On the **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_04.png)

7. <span data-ttu-id="297af-171">V místní nabídce **certifikát výměny** okně klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="297af-171">On the pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="297af-173">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="297af-173">On the **SAML Signing Certificate** section, click **Certificate (base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_05.png) 

9. <span data-ttu-id="297af-175">Na **PlanMyLeave konfigurace** klikněte na tlačítko **konfigurace PlanMyLeave** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="297af-175">On the **PlanMyLeave Configuration** section, click **Configure PlanMyLeave** to open **Configure sign-on** window.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_06.png) 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_07.png)

10. <span data-ttu-id="297af-178">V okně prohlížeče jiný web Přihlaste se ke klientovi PlanMyLeave jako správce.</span><span class="sxs-lookup"><span data-stu-id="297af-178">In a different web browser window, log into your PlanMyLeave tenant as an administrator.</span></span>

11. <span data-ttu-id="297af-179">Přejděte na **nastavení systému**.</span><span class="sxs-lookup"><span data-stu-id="297af-179">Go to **System Setup**.</span></span> <span data-ttu-id="297af-180">Pak na **zabezpečení správy** klikněte na část **nastavení společnosti SAML** .</span><span class="sxs-lookup"><span data-stu-id="297af-180">Then on the **Security Management** section click **Company SAML settings** .</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_002.png) 

12. <span data-ttu-id="297af-182">Na **SAML nastavení** klikněte ikonu editor.</span><span class="sxs-lookup"><span data-stu-id="297af-182">On the **SAML Settings** section, click editor icon.</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_003.png)

13. <span data-ttu-id="297af-184">Na **SAML nastavení aktualizace** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="297af-184">On the **Update SAML Settings** section, perform the following steps:</span></span>

    ![Konfigurace jednotného přihlašování na straně aplikace](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_004.png)

    <span data-ttu-id="297af-186">a.</span><span class="sxs-lookup"><span data-stu-id="297af-186">a.</span></span>  <span data-ttu-id="297af-187">V **přihlašovací adresa URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** z okna konfigurace aplikace Azure AD.</span><span class="sxs-lookup"><span data-stu-id="297af-187">In the **Login URL** textbox, put the value of **SAML Single Sign-On Service URL** from Azure AD application configuration window.</span></span>

    <span data-ttu-id="297af-188">b.</span><span class="sxs-lookup"><span data-stu-id="297af-188">b.</span></span>  <span data-ttu-id="297af-189">Otevřete soubor stažený certifikátu v poznámkovém bloku kopírovat pouze obsah mezi---začít certifikát a---konec certifikátu---ji do schránky a vložte jej do **certifikát** textové pole.</span><span class="sxs-lookup"><span data-stu-id="297af-189">Open your downloaded certificate file in notepad, copy only the content between the ---Begin Certificate--- and ---End certificate---- of it into your clipboard, and then paste it to the **Certificate** textbox.</span></span>

    <span data-ttu-id="297af-190">c.</span><span class="sxs-lookup"><span data-stu-id="297af-190">c.</span></span> <span data-ttu-id="297af-191">Nastavit "**je povolit**"do"**Ano**".</span><span class="sxs-lookup"><span data-stu-id="297af-191">Set "**Is Enable**" to "**Yes**".</span></span>

    <span data-ttu-id="297af-192">d.</span><span class="sxs-lookup"><span data-stu-id="297af-192">d.</span></span> <span data-ttu-id="297af-193">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="297af-193">Click **Save**.</span></span>



### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="297af-194">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="297af-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="297af-195">Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="297af-195">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="297af-197">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="297af-197">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="297af-198">V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="297af-198">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="297af-200">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="297af-200">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="297af-202">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="297af-202">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="297af-204">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="297af-204">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="297af-206">a.</span><span class="sxs-lookup"><span data-stu-id="297af-206">a.</span></span> <span data-ttu-id="297af-207">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="297af-207">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="297af-208">b.</span><span class="sxs-lookup"><span data-stu-id="297af-208">b.</span></span> <span data-ttu-id="297af-209">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="297af-209">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="297af-210">c.</span><span class="sxs-lookup"><span data-stu-id="297af-210">c.</span></span> <span data-ttu-id="297af-211">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="297af-211">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="297af-212">d.</span><span class="sxs-lookup"><span data-stu-id="297af-212">d.</span></span> <span data-ttu-id="297af-213">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="297af-213">Click **Create**.</span></span> 



### <a name="creating-a-planmyleave-test-user"></a><span data-ttu-id="297af-214">Vytvoření zkušebního uživatele PlanMyLeave</span><span class="sxs-lookup"><span data-stu-id="297af-214">Creating a PlanMyLeave test user</span></span>

<span data-ttu-id="297af-215">Cílem této části je vytvoření uživatele v PlanMyLeave nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="297af-215">The objective of this section is to create a user called Britta Simon in PlanMyLeave.</span></span> <span data-ttu-id="297af-216">PlanMyLeave podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="297af-216">PlanMyLeave supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="297af-217">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="297af-217">There is no action item for you in this section.</span></span> <span data-ttu-id="297af-218">Vytvoří se nový uživatel během pokusu o přístup k PlanMyLeave, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="297af-218">A new user will be created during an attempt to access PlanMyLeave if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="297af-219">Pokud potřebujete vytvořit uživatele s ručně, budete muset kontaktovat [tým podpory PlanMyLeave](mailto:support@planmyleave.com).</span><span class="sxs-lookup"><span data-stu-id="297af-219">If you need to create an user manually, you need to contact [PlanMyLeave support team](mailto:support@planmyleave.com).</span></span>



### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="297af-220">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="297af-220">Assigning the Azure AD test user</span></span>

<span data-ttu-id="297af-221">V této části povolíte Britta Simon používat tak, že udělíte přístup k PlanMyLeave Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="297af-221">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to PlanMyLeave.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="297af-223">**Pokud chcete přiřadit Britta Simon PlanMyLeave, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="297af-223">**To assign Britta Simon to PlanMyLeave, perform the following steps:**</span></span>

1. <span data-ttu-id="297af-224">V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="297af-224">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="297af-226">V seznamu aplikací vyberte **PlanMyLeave**.</span><span class="sxs-lookup"><span data-stu-id="297af-226">In the applications list, select **PlanMyLeave**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_50.png) 

3. <span data-ttu-id="297af-228">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="297af-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="297af-230">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="297af-230">Click **Add** button.</span></span> <span data-ttu-id="297af-231">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="297af-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="297af-233">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="297af-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="297af-234">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="297af-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="297af-235">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="297af-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="297af-236">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="297af-236">Testing single sign-on</span></span>

<span data-ttu-id="297af-237">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="297af-237">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="297af-238">Když kliknete na dlaždici PlanMyLeave na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci PlanMyLeave.</span><span class="sxs-lookup"><span data-stu-id="297af-238">When you click the PlanMyLeave tile in the Access Panel, you should get automatically signed-on to your PlanMyLeave application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="297af-239">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="297af-239">Additional resources</span></span>

* [<span data-ttu-id="297af-240">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="297af-240">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="297af-241">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="297af-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_203.png