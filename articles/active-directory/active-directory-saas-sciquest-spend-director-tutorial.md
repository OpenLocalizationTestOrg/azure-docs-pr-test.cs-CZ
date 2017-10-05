---
title: "Kurz: Azure Active Directory integrace s SciQuest tráví ředitel | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SciQuest tráví ředitel."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 9fab641b-292e-4bef-91d1-8ccc4f3a0c1f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 84b707668dc45e92e6151f422f1c919f638533b1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sciquest-spend-director"></a><span data-ttu-id="a063a-103">Kurz: Azure Active Directory integrace s SciQuest tráví ředitel</span><span class="sxs-lookup"><span data-stu-id="a063a-103">Tutorial: Azure Active Directory integration with SciQuest Spend Director</span></span>
<span data-ttu-id="a063a-104">Cílem tohoto kurzu je ukazují, jak integrovat SciQuest tráví ředitel Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a063a-104">The objective of this tutorial is to show you how to integrate SciQuest Spend Director with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="a063a-105">Integrace SciQuest tráví ředitel s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a063a-105">Integrating SciQuest Spend Director with Azure AD provides you with the following benefits:</span></span> 

* <span data-ttu-id="a063a-106">Můžete řídit ve službě Azure AD, který má přístup k SciQuest tráví ředitel</span><span class="sxs-lookup"><span data-stu-id="a063a-106">You can control in Azure AD who has access to SciQuest Spend Director</span></span> 
* <span data-ttu-id="a063a-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k SciQuest tráví ředitel (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a063a-107">You can enable your users to automatically get signed-on to SciQuest Spend Director (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="a063a-108">Můžete spravovat vaše účty v jednom centrálním místě – portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="a063a-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="a063a-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a063a-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a063a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a063a-110">Prerequisites</span></span>
<span data-ttu-id="a063a-111">Konfigurace integrace Azure AD s SciQuest tráví ředitel, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="a063a-111">To configure Azure AD integration with SciQuest Spend Director, you need the following items:</span></span>

* <span data-ttu-id="a063a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a063a-112">An Azure AD subscription</span></span>
* <span data-ttu-id="a063a-113">SciQuest tráví ředitel jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a063a-113">A SciQuest Spend Director single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a063a-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a063a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="a063a-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a063a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="a063a-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="a063a-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="a063a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a063a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="a063a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a063a-118">Scenario Description</span></span>
<span data-ttu-id="a063a-119">Cílem tohoto kurzu je vám umožní testování Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a063a-119">The objective of this tutorial is to enable you to test Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="a063a-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a063a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a063a-121">Přidání SciQuest tráví ředitel z Galerie</span><span class="sxs-lookup"><span data-stu-id="a063a-121">Adding SciQuest Spend Director from the gallery</span></span> 
2. <span data-ttu-id="a063a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a063a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sciquest-spend-director-from-the-gallery"></a><span data-ttu-id="a063a-123">Přidání SciQuest tráví ředitel z Galerie</span><span class="sxs-lookup"><span data-stu-id="a063a-123">Adding SciQuest Spend Director from the gallery</span></span>
<span data-ttu-id="a063a-124">Při konfiguraci integrace SciQuest tráví ředitel do služby Azure AD potřebujete přidat SciQuest tráví ředitel z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="a063a-124">To configure the integration of SciQuest Spend Director into Azure AD, you need to add SciQuest Spend Director from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a063a-125">**Pokud chcete přidat SciQuest tráví ředitel z galerie, postupujte takto:**</span><span class="sxs-lookup"><span data-stu-id="a063a-125">**To add SciQuest Spend Director from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a063a-126">V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="a063a-126">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]

2. <span data-ttu-id="a063a-128">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="a063a-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="a063a-129">Chcete-li otevřít zobrazení aplikací, v zobrazení adresáře, klikněte na tlačítko **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="a063a-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Aplikace][2]

4. <span data-ttu-id="a063a-131">Klikněte na tlačítko **přidat** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="a063a-131">Click **Add** at the bottom of the page.</span></span>
   
    ![Aplikace][3]

5. <span data-ttu-id="a063a-133">Na **co chcete udělat** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="a063a-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Aplikace][4]

6. <span data-ttu-id="a063a-135">Do vyhledávacího pole zadejte **sciQuest tráví ředitel**.</span><span class="sxs-lookup"><span data-stu-id="a063a-135">In the search box, type **sciQuest spend director**.</span></span>
   
    ![Aplikace][5]

7. <span data-ttu-id="a063a-137">V podokně výsledků vyberte **SciQuest tráví ředitel**a potom klikněte na **Complete** tuto aplikaci přidat.</span><span class="sxs-lookup"><span data-stu-id="a063a-137">In the results pane, select **SciQuest Spend Director**, and then click **Complete** to add the application.</span></span>
   
    ![Aplikace][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a063a-139">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a063a-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a063a-140">Cílem této části je ukazují, jak nakonfigurovat a otestovat Azure AD jednotné přihlašování s SciQuest tráví ředitel podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="a063a-140">The objective of this section is to show you how to configure and test Azure AD single sign-on with SciQuest Spend Director based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a063a-141">Pro jednotné přihlašování pro práci je potřeba vědět, co uživatel protějškem v SciQuest tráví ředitel pro uživatele ve službě Azure AD je Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a063a-141">For single sign-on to work, Azure AD needs to know what the counterpart user in SciQuest Spend Director to an user in Azure AD is.</span></span> <span data-ttu-id="a063a-142">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v SciQuest tráví ředitel musí navázat.</span><span class="sxs-lookup"><span data-stu-id="a063a-142">In other words, a link relationship between an Azure AD user and the related user in SciQuest Spend Director needs to be established.</span></span>  
<span data-ttu-id="a063a-143">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v SciQuest tráví ředitel.</span><span class="sxs-lookup"><span data-stu-id="a063a-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in SciQuest Spend Director.</span></span>

<span data-ttu-id="a063a-144">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s SciQuest tráví ředitel, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="a063a-144">To configure and test Azure AD single sign-on with SciQuest Spend Director, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a063a-145">**[Konfigurace Azure AD jeden jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="a063a-145">**[Configuring Azure AD Single Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a063a-146">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a063a-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a063a-147">**[Vytvoření zkušebního uživatele ředitel tráví SciQuest](#creating-a-halogen-software-test-user)**  – Pokud chcete mít protějšek Britta Simon v tráví ředitel SciQuest, propojené služby Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="a063a-147">**[Creating a SciQuest Spend Director test user](#creating-a-halogen-software-test-user)** - to have a counterpart of Britta Simon in SciQuest Spend Director that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="a063a-148">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a063a-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a063a-149">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a063a-149">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-single-sign-on"></a><span data-ttu-id="a063a-150">Konfigurace Azure AD jednoho jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a063a-150">Configuring Azure AD Single Single Sign-On</span></span>
<span data-ttu-id="a063a-151">Cílem této části je chcete povolit Azure AD jednotného přihlašování na portálu Azure classic a nakonfigurovat jednotné přihlašování v aplikaci SciQuest tráví ředitel.</span><span class="sxs-lookup"><span data-stu-id="a063a-151">The objective of this section is to enable Azure AD single sign-on in the Azure classic portal and to configure single sign-on in your SciQuest Spend Director application.</span></span>

<span data-ttu-id="a063a-152">**Ke konfiguraci Azure AD jednotné přihlašování s SciQuest tráví ředitel, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a063a-152">**To configure Azure AD single sign-on with SciQuest Spend Director, perform the following steps:**</span></span>

1. <span data-ttu-id="a063a-153">Na portálu Azure classic na **SciQuest tráví ředitel** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** otevřete **nakonfigurovat jednotné přihlašování**  Dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a063a-153">In the Azure classic portal, on the **SciQuest Spend Director** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurovat jednotné přihlašování][8]

2. <span data-ttu-id="a063a-155">Na **jak chcete uživatelům se přihlásit ředitel tráví SciQuest** vyberte **Azure AD jednotné přihlašování**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a063a-155">On the **How would you like users to sign on to SciQuest Spend Director** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Azure AD jednotné přihlášení][9]

3. <span data-ttu-id="a063a-157">Na **nakonfigurovat nastavení aplikace** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a063a-157">On the **Configure App Settings** dialog page, perform the following steps:</span></span> 
   
    ![Konfigurovat nastavení aplikace][10]
   
     <span data-ttu-id="a063a-159">a.</span><span class="sxs-lookup"><span data-stu-id="a063a-159">a.</span></span> <span data-ttu-id="a063a-160">V **přihlašovací adresa URL** textové pole, zadejte adresu URL používá vaši uživatelé k přihlášení do aplikace tráví ředitel SciQuest pomocí následujícího vzorce: *https://.* SciQuest.com/.**</span><span class="sxs-lookup"><span data-stu-id="a063a-160">In the **Sign On URL** textbox, type your URL used by your users to sign on to your SciQuest Spend Director application using the following pattern: *https://.*sciquest.com/.**</span></span>
   
     <span data-ttu-id="a063a-161">b.</span><span class="sxs-lookup"><span data-stu-id="a063a-161">b.</span></span> <span data-ttu-id="a063a-162">V **adresa URL odpovědi** textovému poli, zadejte stejnou hodnotu, která jste zadali do **přihlašovací adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="a063a-162">In the **Reply URL** textbox, type the same value you have typed into the **Sign On URL** textbox.</span></span> 
   
     <span data-ttu-id="a063a-163">c.</span><span class="sxs-lookup"><span data-stu-id="a063a-163">c.</span></span> <span data-ttu-id="a063a-164">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a063a-164">Click **Next**.</span></span>

4. <span data-ttu-id="a063a-165">Na **nakonfigurovat jednotné přihlašování v ředitel tráví SciQuest** klikněte na tlačítko **stáhnout metadata**a potom uložte soubor metadat místně na vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="a063a-165">On the **Configure single sign-on at SciQuest Spend Director** page, click **Download metadata**, and then save the metadata file locally on your computer.</span></span>
   
    ![Co je služba Azure AD Connect][11]

5. <span data-ttu-id="a063a-167">Kontaktujte SciQuest podporují tuto metodu ověřování pomocí výše uvedených stažené metadat.</span><span class="sxs-lookup"><span data-stu-id="a063a-167">Contact SciQuest support to enable this authentication method using the above downloaded metadata.</span></span>

6. <span data-ttu-id="a063a-168">Na portálu Azure classic, vyberte potvrzení konfigurace přihlášení a pak klikněte na tlačítko **Complete** zavřete **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a063a-168">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span> 
   
    ![Co je služba Azure AD Connect][15]

7. <span data-ttu-id="a063a-170">Na **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="a063a-170">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a063a-171">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a063a-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="a063a-172">Cílem této části je vytvoření zkušebního uživatele na portálu Azure classic názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a063a-172">The objective of this section is to create a test user in the Azure classic portal called Britta Simon.</span></span>

<span data-ttu-id="a063a-173">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a063a-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a063a-174">V **portál Azure classic**, v levém navigačním podokně klikněte na tlačítko **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="a063a-174">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Co je služba Azure AD Connect][100] 

2. <span data-ttu-id="a063a-176">Z **Directory** seznamu, vyberte adresář, pro který chcete povolit integraci adresáře.</span><span class="sxs-lookup"><span data-stu-id="a063a-176">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="a063a-177">Chcete-li zobrazit seznam uživatelů, v nabídce v horní části, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a063a-177">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Co je služba Azure AD Connect][101] 

4. <span data-ttu-id="a063a-179">Chcete-li otevřít **přidat uživatele** dialogovém okně, na panelu nástrojů v dolní části, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="a063a-179">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span> 
   
    ![Co je služba Azure AD Connect][102] 

5. <span data-ttu-id="a063a-181">Na **Povězte nám o tohoto uživatele** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a063a-181">On the **Tell us about this user** dialog page, perform the following steps:</span></span>
   
    ![Co je služba Azure AD Connect][103] 
   
    <span data-ttu-id="a063a-183">a.</span><span class="sxs-lookup"><span data-stu-id="a063a-183">a.</span></span> <span data-ttu-id="a063a-184">Jako **typ uživatele**, vyberte **nového uživatele ve vaší organizaci**.</span><span class="sxs-lookup"><span data-stu-id="a063a-184">As **Type Of User**, select **New user in your organization**.</span></span>
   
    <span data-ttu-id="a063a-185">b.</span><span class="sxs-lookup"><span data-stu-id="a063a-185">b.</span></span> <span data-ttu-id="a063a-186">V uživatelské jméno **textbox**, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a063a-186">In the User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="a063a-187">c.</span><span class="sxs-lookup"><span data-stu-id="a063a-187">c.</span></span> <span data-ttu-id="a063a-188">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a063a-188">Click **Next**.</span></span>

6. <span data-ttu-id="a063a-189">Na **profil uživatele** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a063a-189">On the **User Profile** dialog page, perform the following steps:</span></span> 
   
    ![Co je služba Azure AD Connect][104] 
   
    <span data-ttu-id="a063a-191">a.</span><span class="sxs-lookup"><span data-stu-id="a063a-191">a.</span></span> <span data-ttu-id="a063a-192">V **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="a063a-192">In the **First Name** textbox, type **Britta**.</span></span>  
   
    <span data-ttu-id="a063a-193">b.</span><span class="sxs-lookup"><span data-stu-id="a063a-193">b.</span></span> <span data-ttu-id="a063a-194">V **příjmení** txtbox, typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="a063a-194">In the **Last Name** txtbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="a063a-195">c.</span><span class="sxs-lookup"><span data-stu-id="a063a-195">c.</span></span> <span data-ttu-id="a063a-196">V **zobrazovaný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="a063a-196">In the **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="a063a-197">d.</span><span class="sxs-lookup"><span data-stu-id="a063a-197">d.</span></span> <span data-ttu-id="a063a-198">V **Role** seznamu, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="a063a-198">In the **Role** list, select **User**.</span></span>
   
    <span data-ttu-id="a063a-199">e.</span><span class="sxs-lookup"><span data-stu-id="a063a-199">e.</span></span> <span data-ttu-id="a063a-200">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a063a-200">Click **Next**.</span></span>

7. <span data-ttu-id="a063a-201">Na **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a063a-201">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Co je služba Azure AD Connect][105]  

8. <span data-ttu-id="a063a-203">Na **získat dočasné heslo** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a063a-203">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Co je služba Azure AD Connect][106]   
   
    <span data-ttu-id="a063a-205">a.</span><span class="sxs-lookup"><span data-stu-id="a063a-205">a.</span></span> <span data-ttu-id="a063a-206">Poznamenejte si hodnotu **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="a063a-206">Write down the value of the **New Password**.</span></span>
   
    <span data-ttu-id="a063a-207">b.</span><span class="sxs-lookup"><span data-stu-id="a063a-207">b.</span></span> <span data-ttu-id="a063a-208">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="a063a-208">Click **Complete**.</span></span>   

### <a name="creating-a-sciquest-spend-director-test-user"></a><span data-ttu-id="a063a-209">Vytvoření zkušebního uživatele SciQuest tráví ředitel</span><span class="sxs-lookup"><span data-stu-id="a063a-209">Creating a SciQuest Spend Director test user</span></span>
<span data-ttu-id="a063a-210">Cílem této části je vytvoření uživatele volal Britta Simon v SciQuest tráví ředitel.</span><span class="sxs-lookup"><span data-stu-id="a063a-210">The objective of this section is to create a user called Britta Simon in SciQuest Spend Director.</span></span>

<span data-ttu-id="a063a-211">Budete muset kontaktujte tým podpory SciQuest tráví ředitele a poskytněte s podrobnostmi o vašem účtu testovací získat vytvořen.</span><span class="sxs-lookup"><span data-stu-id="a063a-211">You need to contact your SciQuest Spend Director support team and provide them with the details about your test account to get it created.</span></span>

<span data-ttu-id="a063a-212">Alternativně můžete využít i za běhu zřizování, jeden přihlašování funkce, která podporuje SciQuest tráví ředitel.</span><span class="sxs-lookup"><span data-stu-id="a063a-212">Alternatively, you can also leverage just-in-time provisioning, a single sign-on feature that is supported by SciQuest Spend Director.</span></span>  
<span data-ttu-id="a063a-213">Pokud za běhu zřizování je povolená, uživatelé jsou automaticky vytváří SciQuest tráví ředitel během jednoho pokusu o přihlášení Pokud ještě neexistují.</span><span class="sxs-lookup"><span data-stu-id="a063a-213">If just-in-time provisioning is enabled, users are automatically created by SciQuest Spend Director during a single sign-on attempt if they don't exist.</span></span> <span data-ttu-id="a063a-214">Tato funkce eliminuje nutnost ručně vytvořit protějšku přihlašování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a063a-214">This feature eliminates the need to manually create single sign-on counterpart users.</span></span>

<span data-ttu-id="a063a-215">Pokud chcete získat za běhu zřizování povolena, je potřeba obraťte se na to, co váš tým podpory SciQuest tráví ředitel.</span><span class="sxs-lookup"><span data-stu-id="a063a-215">To get just-in-time provisioning enabled, you need to contact your your SciQuest Spend Director support team.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a063a-216">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a063a-216">Assigning the Azure AD test user</span></span>
<span data-ttu-id="a063a-217">Cílem této části je povolení Britta Simon používat tak, že udělíte přístup k SciQuest tráví ředitel Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a063a-217">The objective of this section is to enabling Britta Simon to use Azure single sign-on by granting her access to SciQuest Spend Director.</span></span>

![Co je služba Azure AD Connect][200]

<span data-ttu-id="a063a-219">**Pokud chcete přiřadit Britta Simon SciQuest tráví ředitel, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a063a-219">**To assign Britta Simon to SciQuest Spend Director, perform the following steps:**</span></span>

1. <span data-ttu-id="a063a-220">Na portálu Azure classic, otevřete zobrazení aplikací, v zobrazení adresáře, klikněte na **aplikace** v horní nabídce.</span><span class="sxs-lookup"><span data-stu-id="a063a-220">On the Azure classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Co je služba Azure AD Connect][201]

2. <span data-ttu-id="a063a-222">V seznamu aplikací vyberte **SciQuest tráví ředitel**.</span><span class="sxs-lookup"><span data-stu-id="a063a-222">In the applications list, select **SciQuest Spend Director**.</span></span>
   
    ![Co je služba Azure AD Connect][202]

3. <span data-ttu-id="a063a-224">V nabídce v horní části, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a063a-224">In the menu on the top, click **Users**.</span></span>
   
    ![Co je služba Azure AD Connect][203]

4. <span data-ttu-id="a063a-226">V seznamu uživatelů vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="a063a-226">In the Users list, select **Britta Simon**.</span></span>
   
    ![Co je služba Azure AD Connect][204]

5. <span data-ttu-id="a063a-228">Na panelu nástrojů v dolní části klikněte na tlačítko **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="a063a-228">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Co je služba Azure AD Connect][205]

### <a name="testing-single-sign-on"></a><span data-ttu-id="a063a-230">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a063a-230">Testing Single Sign-On</span></span>
<span data-ttu-id="a063a-231">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a063a-231">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="a063a-232">Když kliknete na dlaždici SciQuest tráví ředitel na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci SciQuest tráví ředitel.</span><span class="sxs-lookup"><span data-stu-id="a063a-232">When you click the SciQuest Spend Director tile in the Access Panel, you should get automatically signed-on to your SciQuest Spend Director application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a063a-233">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a063a-233">Additional Resources</span></span>
* [<span data-ttu-id="a063a-234">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a063a-234">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a063a-235">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a063a-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_20.png

