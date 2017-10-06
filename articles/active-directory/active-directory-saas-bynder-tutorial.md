---
title: 'Kurz: Azure Active Directory integrace s Bynder | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Bynder."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 4fb0ab26-b3b9-420a-8072-a0be80ea021e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: a2a8477580d28fe422f2836f483dff286bc71c93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bynder"></a><span data-ttu-id="a8f24-103">Kurz: Azure Active Directory integrace s Bynder</span><span class="sxs-lookup"><span data-stu-id="a8f24-103">Tutorial: Azure Active Directory integration with Bynder</span></span>
<span data-ttu-id="a8f24-104">cílem Hello tohoto kurzu je tooshow můžete jak toointegrate Bynder s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a8f24-104">hello objective of this tutorial is tooshow you how toointegrate Bynder with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a8f24-105">Integrace Bynder s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a8f24-105">Integrating Bynder with Azure AD provides you with hello following benefits:</span></span>

* <span data-ttu-id="a8f24-106">Můžete řídit ve službě Azure AD, který má přístup tooBynder</span><span class="sxs-lookup"><span data-stu-id="a8f24-106">You can control in Azure AD who has access tooBynder</span></span>
* <span data-ttu-id="a8f24-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooBynder jednotné přihlašování (SSO) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8f24-107">You can enable your users tooautomatically get signed-on tooBynder single sign-on (SSO) with their Azure AD accounts</span></span>
* <span data-ttu-id="a8f24-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="a8f24-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="a8f24-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a8f24-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8f24-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a8f24-110">Prerequisites</span></span>
<span data-ttu-id="a8f24-111">Integrace služby Azure AD s Bynder tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="a8f24-111">tooconfigure Azure AD integration with Bynder, you need hello following items:</span></span>

* <span data-ttu-id="a8f24-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8f24-112">An Azure AD subscription</span></span>
* <span data-ttu-id="a8f24-113">Předplatné povolené Bynder jednotného přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="a8f24-113">A Bynder single-sign on (SSO) enabled subscription</span></span>

>[!NOTE]
><span data-ttu-id="a8f24-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a8f24-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span> 
> 

<span data-ttu-id="a8f24-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a8f24-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="a8f24-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="a8f24-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="a8f24-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a8f24-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a8f24-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a8f24-118">Scenario description</span></span>
<span data-ttu-id="a8f24-119">cílem Hello tohoto kurzu je tooenable jste tootest Microsoft Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a8f24-119">hello objective of this tutorial is tooenable you tootest Microsoft Azure AD SSO in a test environment.</span></span>

<span data-ttu-id="a8f24-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a8f24-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a8f24-121">Přidání Bynder z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="a8f24-121">Adding Bynder from hello gallery</span></span>
2. <span data-ttu-id="a8f24-122">Konfigurace a testování jednotného přihlašování pro aplikaci Microsoft Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8f24-122">Configuring and testing Microsoft Azure AD SSO</span></span>

## <a name="add-bynder-from-hello-gallery"></a><span data-ttu-id="a8f24-123">Přidat Bynder z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="a8f24-123">Add Bynder from hello gallery</span></span>
<span data-ttu-id="a8f24-124">tooconfigure hello integrace Bynder do Azure AD, je nutné tooadd Bynder hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="a8f24-124">tooconfigure hello integration of Bynder into Azure AD, you need tooadd Bynder from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a8f24-125">**tooadd Bynder z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a8f24-125">**tooadd Bynder from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8f24-126">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-126">In hello **Azure classic Portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]
2. <span data-ttu-id="a8f24-128">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="a8f24-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="a8f24-129">Klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="a8f24-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Aplikace][2]
4. <span data-ttu-id="a8f24-131">Klikněte na tlačítko **přidat** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="a8f24-131">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Aplikace][3]
5. <span data-ttu-id="a8f24-133">Na hello **co chcete toodo** dialogové okno, klikněte na tlačítko **přidat aplikaci z Galerie hello**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Aplikace][4]
6. <span data-ttu-id="a8f24-135">Hello vyhledávacího pole zadejte **Bynder**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-135">In hello search box, type **Bynder**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_01.png)
7. <span data-ttu-id="a8f24-137">Na panelu výsledků hello vyberte **Bynder**a potom klikněte na **Complete** tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="a8f24-137">In hello results panel, select **Bynder**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Výběr aplikace hello v galerii hello](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_001.png)

## <a name="configure-and-test-microsoft-azure-ad-sso"></a><span data-ttu-id="a8f24-139">Konfigurace a otestování jednotného přihlašování pro aplikaci Microsoft Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8f24-139">Configure and test Microsoft Azure AD SSO</span></span>
<span data-ttu-id="a8f24-140">Hello cílem této části je tooshow vám jak tooconfigure a testovací Microsoft Azure AD přihlášení SSO se Bynder na základě testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="a8f24-140">hello objective of this section is tooshow you how tooconfigure and test Microsoft Azure AD SSO with Bynder based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a8f24-141">Pro jednotné přihlašování toowork Azure AD musí tooknow, co uživatel hello protějškem v Bynder tooan uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8f24-141">For SSO toowork, Azure AD needs tooknow what hello counterpart user in Bynder tooan user in Azure AD is.</span></span> <span data-ttu-id="a8f24-142">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Bynder musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="a8f24-142">In other words, a link relationship between an Azure AD user and hello related user in Bynder needs toobe established.</span></span>

<span data-ttu-id="a8f24-143">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Bynder.</span><span class="sxs-lookup"><span data-stu-id="a8f24-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Bynder.</span></span>

<span data-ttu-id="a8f24-144">tooconfigure a testovací Microsoft Azure AD přihlášení SSO se Bynder, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="a8f24-144">tooconfigure and test Microsoft Azure AD SSO with Bynder, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a8f24-145">**[Konfigurace Microsoft Azure AD jednotné přihlašování](#configuring-azure-ad-single-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="a8f24-145">**[Configuring Microsoft Azure AD single sign-on](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a8f24-146">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Microsoft Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a8f24-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Microsoft Azure AD Single Sign-On with Britta Simon.</span></span>
3. <span data-ttu-id="a8f24-147">**[Vytvoření zkušebního uživatele Bynder](#creating-a-bynder-test-user)**  -toohave protějšek Britta Simon v Bynder, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8f24-147">**[Creating a Bynder test user](#creating-a-bynder-test-user)** - toohave a counterpart of Britta Simon in Bynder that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="a8f24-148">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable toouse Britta Simon Microsoft Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a8f24-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Microsoft Azure AD Single Sign-On.</span></span>
5. <span data-ttu-id="a8f24-149">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="a8f24-149">**[Testing single sign-on](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-microsoft-azure-ad-sso"></a><span data-ttu-id="a8f24-150">Konfigurace jednotného přihlašování Microsoft Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8f24-150">Configuring Microsoft Azure AD SSO</span></span>
<span data-ttu-id="a8f24-151">V této části můžete povolit jednotné přihlašování pro aplikaci Microsoft Azure AD na portálu classic hello a nakonfigurovat jednotné přihlašování v aplikaci Bynder.</span><span class="sxs-lookup"><span data-stu-id="a8f24-151">In this section, you enable Microsoft Azure AD SSO in hello classic portal and configure SSO in your Bynder application.</span></span>

<span data-ttu-id="a8f24-152">**tooconfigure Microsoft Azure AD přihlášení SSO se Bynder, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a8f24-152">**tooconfigure Microsoft Azure AD SSO with Bynder, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8f24-153">Na portálu classic hello na hello **Bynder** stránky integrace aplikací, klikněte na tlačítko **nakonfigurovat jednotné přihlašování** tooopen hello **nakonfigurovat jednotné přihlašování** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a8f24-153">In hello classic portal, on hello **Bynder** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurovat jednotné přihlašování][6] 
2. <span data-ttu-id="a8f24-155">Na hello **jak jste by například uživatelé toosign na tooBynder** vyberte **Microsoft Azure AD Single Sign-On**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-155">On hello **How would you like users toosign on tooBynder** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_03.png)
3. <span data-ttu-id="a8f24-157">Na hello **nakonfigurovat nastavení aplikace** dialogové okno stránky, pokud chcete aplikace hello tooconfigure v **IDP iniciované režimu**, proveďte následující kroky hello a klikněte na tlačítko **Další**:</span><span class="sxs-lookup"><span data-stu-id="a8f24-157">On hello **Configure App Settings** dialog page, If you wish tooconfigure hello application in **IDP initiated mode**, perform hello following steps and click **Next**:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_04.png)
  1. <span data-ttu-id="a8f24-159">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.getbynder.com/sso/SAML/authenticate/`</span><span class="sxs-lookup"><span data-stu-id="a8f24-159">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.getbynder.com/sso/SAML/authenticate/`</span></span>
  2. <span data-ttu-id="a8f24-160">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-160">Click **Next**.</span></span>
4. <span data-ttu-id="a8f24-161">Pokud chcete aplikace hello tooconfigure v **SP iniciované režimu** na hello **nakonfigurovat nastavení aplikace** stránku dialogové okno a potom klikněte na hello **"Zobrazit rozšířená nastavení (volitelné)"**a pak zadejte hello **přihlašovací adresa URL** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-161">If you wish tooconfigure hello application in **SP initiated mode** on hello **Configure App Settings** dialog page, then click on hello **“Show advanced settings (optional)”** and then enter hello **Sign On URL** and click **Next**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_10.png)
  1. <span data-ttu-id="a8f24-163">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<company name>.getbynder.com/login/`</span><span class="sxs-lookup"><span data-stu-id="a8f24-163">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://<company name>.getbynder.com/login/`</span></span>
  2. <span data-ttu-id="a8f24-164">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-164">Click **Next**.</span></span>
  
   >[!NOTE]
   ><span data-ttu-id="a8f24-165">Hodnota Hello hello přihlašovací adresa URL v tomto kurzu je právě placeholfer.</span><span class="sxs-lookup"><span data-stu-id="a8f24-165">hello value for hello Sign On URL in this tutorial is just a placeholfer.</span></span> <span data-ttu-id="a8f24-166">tooget hello skutečné vlaue pro vaše prostředí, obraťte se na Bynder.</span><span class="sxs-lookup"><span data-stu-id="a8f24-166">tooget hello actual vlaue for your environment, contact Bynder.</span></span>
   >

5. <span data-ttu-id="a8f24-167">Na hello **nakonfigurovat jednotné přihlašování v Bynder** , proveďte následující kroky hello a klikněte na tlačítko **Další**:</span><span class="sxs-lookup"><span data-stu-id="a8f24-167">On hello **Configure single sign-on at Bynder** page, perform hello following steps and click **Next**:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_05.png)  
  1. <span data-ttu-id="a8f24-169">Klikněte na tlačítko **stáhnout metadata**a potom uložte soubor hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="a8f24-169">Click **Download metadata**, and then save hello file on your computer.</span></span>
  2. <span data-ttu-id="a8f24-170">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-170">Click **Next**.</span></span>
6. <span data-ttu-id="a8f24-171">tooget jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, kontaktujte tým podpory Bynder.</span><span class="sxs-lookup"><span data-stu-id="a8f24-171">tooget SSO configured for your application, contact your Bynder support team.</span></span> <span data-ttu-id="a8f24-172">Připojte soubor stažený metadat hello a sdílet s Bynder team tooset až jednotného přihlašování na jejich straně.</span><span class="sxs-lookup"><span data-stu-id="a8f24-172">Attach hello downloaded metadata file and share it with Bynder team tooset up SSO on their side.</span></span>
7. <span data-ttu-id="a8f24-173">Hello portálu classic, vyberte hello potvrzení konfigurace přihlášení a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-173">In hello classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span>
   
    ![Azure AD jednotné přihlášení][10]
8. <span data-ttu-id="a8f24-175">Na hello **jednotné přihlašování potvrzení** klikněte na tlačítko **Complete**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-175">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
   
    ![Azure AD jednotné přihlášení][11]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="a8f24-177">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8f24-177">Create an Azure AD test user</span></span>
<span data-ttu-id="a8f24-178">Hello cílem této části je toocreate testovacího uživatele na portálu classic hello názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a8f24-178">hello objective of this section is toocreate a test user in hello classic portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][20]

<span data-ttu-id="a8f24-180">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a8f24-180">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8f24-181">V hello **portál Azure classic**, na levém navigačním podokně text hello, klikněte na **služby Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-181">In hello **Azure classic Portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_09.png)
2. <span data-ttu-id="a8f24-183">Z hello **Directory** seznamu, vyberte hello adresář, pro které chcete tooenable integrace adresáře.</span><span class="sxs-lookup"><span data-stu-id="a8f24-183">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="a8f24-184">Klikněte na tlačítko toodisplay hello seznam uživatelů, v nabídce hello hello nahoře **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-184">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_03.png)
4. <span data-ttu-id="a8f24-186">tooopen hello **přidat uživatele** dialogové okno, ve hello nástrojů v dolní části hello, klikněte na tlačítko **přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-186">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_04.png)
5. <span data-ttu-id="a8f24-188">Na hello **Povězte nám o tohoto uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a8f24-188">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_05.png)
  1. <span data-ttu-id="a8f24-190">Jako typ uživatele vyberte nového uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="a8f24-190">As Type Of User, select New user in your organization.</span></span>
  2. <span data-ttu-id="a8f24-191">V hello uživatelské jméno **textbox**, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-191">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
  3. <span data-ttu-id="a8f24-192">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-192">Click **Next**.</span></span>
6. <span data-ttu-id="a8f24-193">Na hello **profil uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a8f24-193">On hello **User Profile** dialog page, perform hello following steps:</span></span>
   
   ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_06.png)
  1. <span data-ttu-id="a8f24-195">V hello **křestní jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-195">In hello **First Name** textbox, type **Britta**.</span></span>  
  2. <span data-ttu-id="a8f24-196">V hello **příjmení** textovému poli, typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-196">In hello **Last Name** textbox, type, **Simon**.</span></span> 
  3. <span data-ttu-id="a8f24-197">V hello **zobrazovaný název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-197">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
  4. <span data-ttu-id="a8f24-198">V hello **Role** seznamu, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-198">In hello **Role** list, select **User**.</span></span>
  5. <span data-ttu-id="a8f24-199">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-199">Click **Next**.</span></span>
7. <span data-ttu-id="a8f24-200">Na hello **získat dočasné heslo** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-200">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_07.png)
8. <span data-ttu-id="a8f24-202">Na hello **získat dočasné heslo** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a8f24-202">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_08.png)
   1. <span data-ttu-id="a8f24-204">Poznamenejte si hodnotu hello hello **nové heslo**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-204">Write down hello value of hello **New Password**.</span></span>
   2. <span data-ttu-id="a8f24-205">Klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-205">Click **Complete**.</span></span>   

### <a name="create-a-bynder-test-user"></a><span data-ttu-id="a8f24-206">Vytvoření zkušebního uživatele Bynder</span><span class="sxs-lookup"><span data-stu-id="a8f24-206">Create a Bynder test user</span></span>
<span data-ttu-id="a8f24-207">Hello cílem této části je toocreate volal Britta Simon v Bynder uživatele.</span><span class="sxs-lookup"><span data-stu-id="a8f24-207">hello objective of this section is toocreate a user called Britta Simon in Bynder.</span></span> <span data-ttu-id="a8f24-208">Bynder podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="a8f24-208">Bynder supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="a8f24-209">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="a8f24-209">There is no action item for you in this section.</span></span> <span data-ttu-id="a8f24-210">Pokud ještě neexistuje, se během pokusu o tooaccess Bynder vytvoří nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="a8f24-210">A new user will be created during an attempt tooaccess Bynder if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="a8f24-211">Pokud potřebujete toocreate uživatelé ručně, je nutné tým podpory Bynder toocontact hello.</span><span class="sxs-lookup"><span data-stu-id="a8f24-211">If you need toocreate an user manually, you need toocontact hello Bynder support team.</span></span> 
> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="a8f24-212">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="a8f24-212">Assign hello Azure AD test user</span></span>
<span data-ttu-id="a8f24-213">Hello cílem této části je tooenabling toouse Britta Simon jednotného přihlašování k Azure tak, že udělíte jeho tooBynder přístup.</span><span class="sxs-lookup"><span data-stu-id="a8f24-213">hello objective of this section is tooenabling Britta Simon toouse Azure SSO by granting her access tooBynder.</span></span>

   ![Přiřadit uživatele][200]

<span data-ttu-id="a8f24-215">**tooassign Britta Simon tooBynder, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="a8f24-215">**tooassign Britta Simon tooBynder, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8f24-216">Na portálu classic hello, klikněte na zobrazení aplikace hello tooopen, v zobrazení adresáře hello **aplikace** v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="a8f24-216">On hello classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Přiřadit uživatele][201]
2. <span data-ttu-id="a8f24-218">V seznamu aplikace hello vyberte **Bynder**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-218">In hello applications list, select **Bynder**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_50.png)
3. <span data-ttu-id="a8f24-220">V nabídce hello hello nahoře, klikněte na tlačítko **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-220">In hello menu on hello top, click **Users**.</span></span>
   
    ![Přiřadit uživatele][203]
4. <span data-ttu-id="a8f24-222">V seznamu uživatelé hello vyberte **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-222">In hello Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="a8f24-223">V panelu nástrojů hello na dolní hello, klikněte na **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="a8f24-223">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Přiřadit uživatele][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="a8f24-225">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a8f24-225">Test single sign-on</span></span>
<span data-ttu-id="a8f24-226">Hello cílem této části je tootest pomocí konfigurace Microsoft Azure AD SSO hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a8f24-226">hello objective of this section is tootest your Microsoft Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="a8f24-227">Když kliknete na dlaždici Bynder hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Bynder aplikace.</span><span class="sxs-lookup"><span data-stu-id="a8f24-227">When you click hello Bynder tile in hello Access Panel, you should get automatically signed-on tooyour Bynder application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a8f24-228">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a8f24-228">Additional resources</span></span>
* [<span data-ttu-id="a8f24-229">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a8f24-229">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a8f24-230">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a8f24-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_205.png
