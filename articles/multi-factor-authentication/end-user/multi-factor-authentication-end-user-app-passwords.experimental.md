---
title: "aaaHow toouse hesla aplikací v Azure MFA? | Dokumentace Microsoftu"
description: "Tato stránka vám pomůže pochopit, jaké jsou hesla aplikací a jaké jsou uživatelé použít pro s ohledem tooAzure vícefaktorového ověřování."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: kgremban
ms.custom: end-user
ms.openlocfilehash: 3afa2003d8e87576f035bf9440a1dba67bd85f5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a><span data-ttu-id="30c85-104">Co jsou hesla aplikací v Azure Multi-Factor Authentication?</span><span class="sxs-lookup"><span data-stu-id="30c85-104">What are App Passwords in Azure Multi-Factor Authentication?</span></span>
<span data-ttu-id="30c85-105">Některé neprohlížečové aplikace, jako je například hello Apple nativní e-mailové klienta, který používá protokolu Exchange Active Sync, aktuálně nepodporují službu Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="30c85-105">Certain non-browser apps, such as hello Apple native email client that uses Exchange Active Sync, currently do not support multi-factor authentication.</span></span> <span data-ttu-id="30c85-106">Služba Multi-Factor authentication je povoleno podle uživatelů.</span><span class="sxs-lookup"><span data-stu-id="30c85-106">Multi-factor authentication is enabled per user.</span></span>  <span data-ttu-id="30c85-107">To znamená, že uživatel nemůže používání služby Multi-Factor authentication, pokud:</span><span class="sxs-lookup"><span data-stu-id="30c85-107">This means that a user can't use multi-factor authentication if:</span></span>

- <span data-ttu-id="30c85-108">Hello uživatel povolen pro službu Multi-Factor authentication</span><span class="sxs-lookup"><span data-stu-id="30c85-108">hello user has been enabled for multi-factor authentication</span></span>
- <span data-ttu-id="30c85-109">Hello uživatel se pokusil toouse neprohlížečovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="30c85-109">hello user is trying toouse a non-browser app.</span></span>

<span data-ttu-id="30c85-110">Heslo aplikace umožňuje hello uživatele toouse hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="30c85-110">An app password allows hello user toouse hello app.</span></span>

<span data-ttu-id="30c85-111">Až budete mít heslo aplikace, použijte místo původní heslo těchto neprohlížečových aplikací.</span><span class="sxs-lookup"><span data-stu-id="30c85-111">Once you have an app password, you use it in place of your original password with these non-browser apps.</span></span> <span data-ttu-id="30c85-112">Při registraci pro dvoustupňové ověření, jste informuje Microsoft není toolet každý, kdo přihlásit pomocí hesla Pokud nemůžou také provést hello druhé ověření.</span><span class="sxs-lookup"><span data-stu-id="30c85-112">When you register for two-step verification, you're telling Microsoft not toolet anyone sign in with your password if they can't also perform hello second verification.</span></span> <span data-ttu-id="30c85-113">Hello Apple nativní e-mailového klienta na váš telefon nemůžete se přihlásit jako je vzhledem k tomu, že nemůžete žádat pro dvoustupňové ověření.</span><span class="sxs-lookup"><span data-stu-id="30c85-113">hello Apple native email client on your phone can't sign in as you because it can't ask for two-step verification.</span></span> <span data-ttu-id="30c85-114">Hello řešení toothis problém je toocreate bezpečnější heslo aplikace, které nepoužíváte každodenní.</span><span class="sxs-lookup"><span data-stu-id="30c85-114">hello solution toothis problem is toocreate a more secure app password that you don't use day-to-day.</span></span> <span data-ttu-id="30c85-115">Hesla aplikací jsou pouze pro tyto aplikace, které nemohou podporovat dvoustupňové ověřování.</span><span class="sxs-lookup"><span data-stu-id="30c85-115">App passwords are only for those apps that can't support two-step verification.</span></span> <span data-ttu-id="30c85-116">Používejte heslo aplikace hello, takže můžete přeskočit službu Multi-Factor authentication a pokračovat toowork aplikace.</span><span class="sxs-lookup"><span data-stu-id="30c85-116">Use hello app password so that apps can bypass multi-factor authentication and continue toowork.</span></span>


> [!NOTE]
> <span data-ttu-id="30c85-117">Klienti Office 2013 (včetně Outlook) podporují nové protokoly pro ověřování a lze použít s dvoustupňové ověřování.</span><span class="sxs-lookup"><span data-stu-id="30c85-117">Office 2013 clients (including Outlook) support new authentication protocols and can be used with two-step verification.</span></span> <span data-ttu-id="30c85-118">Hesla aplikací nejsou potřeba použít s klienty Office 2013.</span><span class="sxs-lookup"><span data-stu-id="30c85-118">App passwords are not required for use with Office 2013 clients.</span></span>  <span data-ttu-id="30c85-119">Další informace najdete v tématu [Office 2013 veřejné Preview verze moderního ověřování oznámeno](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).</span><span class="sxs-lookup"><span data-stu-id="30c85-119">For more information, see [Office 2013 modern authentication public preview announced](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).</span></span>


## <a name="how-toouse-app-passwords"></a><span data-ttu-id="30c85-120">Jak toouse hesel aplikací</span><span class="sxs-lookup"><span data-stu-id="30c85-120">How toouse app passwords</span></span>
<span data-ttu-id="30c85-121">Zde jsou některé věci tooknow o heslech aplikací:</span><span class="sxs-lookup"><span data-stu-id="30c85-121">Here are some things tooknow about app passwords:</span></span>

* <span data-ttu-id="30c85-122">Nevytvářejte vlastní hesla aplikací.</span><span class="sxs-lookup"><span data-stu-id="30c85-122">You don't create your own app passwords.</span></span> <span data-ttu-id="30c85-123">Jsou automaticky generovány.</span><span class="sxs-lookup"><span data-stu-id="30c85-123">They are automatically generated.</span></span>
* <span data-ttu-id="30c85-124">Aktuálně je omezení 40 hesel na uživatele.</span><span class="sxs-lookup"><span data-stu-id="30c85-124">Currently there is a limit of 40 passwords per user.</span></span> 
* <span data-ttu-id="30c85-125">Pokud po dosažení limitu hello toocreate heslo aplikace, budete mít toodelete jeden z vašich dosavadních hesel aplikací předtím, než vytvoříte nový.</span><span class="sxs-lookup"><span data-stu-id="30c85-125">If you try toocreate an app password after you have reached hello limit, you'll have toodelete one of your existing app passwords before you create a new one.</span></span>
* <span data-ttu-id="30c85-126">Používejte jedno heslo aplikace na zařízení, nikoliv podle aplikací.</span><span class="sxs-lookup"><span data-stu-id="30c85-126">Use one app password per device, not per application.</span></span> <span data-ttu-id="30c85-127">Můžete například vytvořit jedno heslo aplikace pro přenosný počítač a používat takové heslo aplikace pro všechny aplikace v tomto přenosném počítači.</span><span class="sxs-lookup"><span data-stu-id="30c85-127">For example, you can create one app password for your laptop and use that app password for all of your applications on that laptop.</span></span> <span data-ttu-id="30c85-128">Pak vytvořte druhý toouse heslo aplikace pro všechny aplikace na ploše.</span><span class="sxs-lookup"><span data-stu-id="30c85-128">Then, create a second app password toouse for all your apps on your desktop.</span></span> 
* <span data-ttu-id="30c85-129">Máte jeden hello heslo aplikace při prvním zaregistrujete pro dvoustupňové ověření.</span><span class="sxs-lookup"><span data-stu-id="30c85-129">You are given one app password hello first time you register for two-step verification.</span></span>  <span data-ttu-id="30c85-130">Pokud potřebujete další těch, můžete je vytvořit.</span><span class="sxs-lookup"><span data-stu-id="30c85-130">If you need additional ones, you can create them.</span></span>



## <a name="creating-and-deleting-app-passwords"></a><span data-ttu-id="30c85-131">Vytvoření nebo odstranění hesla aplikace</span><span class="sxs-lookup"><span data-stu-id="30c85-131">Creating and deleting app passwords</span></span>
<span data-ttu-id="30c85-132">Během vaše počáteční přihlášení budete mít heslo aplikace, kterou můžete použít.</span><span class="sxs-lookup"><span data-stu-id="30c85-132">During your initial sign-in, you are given an app password that you can use.</span></span>  <span data-ttu-id="30c85-133">Můžete také vytvořit a později odstranit hesla aplikací.</span><span class="sxs-lookup"><span data-stu-id="30c85-133">You can also create and delete app passwords later on.</span></span> <span data-ttu-id="30c85-134">Jak odstranit hesla aplikací závisí na tom, jak používat ověřování Multi-Factor authentication.</span><span class="sxs-lookup"><span data-stu-id="30c85-134">How you delete app passwords depends on how you use multi-factor authentication.</span></span> <span data-ttu-id="30c85-135">Odpověď hello následující otázky toodetermine, kde by měli přejít toomanage hesla aplikací:</span><span class="sxs-lookup"><span data-stu-id="30c85-135">Answer hello following questions toodetermine where you should go toomanage app passwords:</span></span> 

1. <span data-ttu-id="30c85-136">Používáte dvoustupňové ověřování pro svůj osobní účet Microsoft?</span><span class="sxs-lookup"><span data-stu-id="30c85-136">Do you use two-step verification for your personal Microsoft account?</span></span> <span data-ttu-id="30c85-137">Pokud ano, by měla odkazovat toohello [hesla aplikací a dvoustupňové ověření](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) článek nápovědy.</span><span class="sxs-lookup"><span data-stu-id="30c85-137">If yes, you should refer toohello [App passwords and two-step verification](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) article for help.</span></span> <span data-ttu-id="30c85-138">Pokud ne, pokračujte tooquestion dva.</span><span class="sxs-lookup"><span data-stu-id="30c85-138">If no, continue tooquestion two.</span></span>

2. <span data-ttu-id="30c85-139">OK, abyste používat dvoustupňové ověřování pro svůj pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="30c85-139">Ok, so you use two-step verification for your work or school account.</span></span> <span data-ttu-id="30c85-140">Používáte ji toosign v aplikacích tooOffice 365?</span><span class="sxs-lookup"><span data-stu-id="30c85-140">Do you use it toosign in tooOffice 365 apps?</span></span> <span data-ttu-id="30c85-141">Pokud ano, se seznamte s příliš[vytvořit heslo aplikace pro Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) nápovědu.</span><span class="sxs-lookup"><span data-stu-id="30c85-141">If yes, you should refer too[Create an app password for Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) for help.</span></span> <span data-ttu-id="30c85-142">Pokud ne, pokračujte tooquestion tři.</span><span class="sxs-lookup"><span data-stu-id="30c85-142">If no, continue tooquestion three.</span></span> 

3. <span data-ttu-id="30c85-143">Používáte dvoustupňové ověření pomocí Microsoft Azure?</span><span class="sxs-lookup"><span data-stu-id="30c85-143">Do you use two-step verification with Microsoft Azure?</span></span> <span data-ttu-id="30c85-144">Pokud ano, pokračovat toohello [spravovat hesla aplikace v portálu Azure hello](#manage-app-passwords-in-the-Azure-portal) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="30c85-144">If yes, continue toohello [Manage app passwords in hello Azure portal](#manage-app-passwords-in-the-Azure-portal) section of this article.</span></span> <span data-ttu-id="30c85-145">Pokud ne, pokračujte tooquestion čtyři.</span><span class="sxs-lookup"><span data-stu-id="30c85-145">If no, continue tooquestion four.</span></span>

4. <span data-ttu-id="30c85-146">Nejste si jisti, kde používáte dvoustupňové ověřování?</span><span class="sxs-lookup"><span data-stu-id="30c85-146">Not sure where you use two-step verification?</span></span> <span data-ttu-id="30c85-147">Pokračovat toohello [spravovat hesla aplikací s hello MyApps portál](#manage-app-passwords-with-the-myapps-portal) tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="30c85-147">Continue toohello [Manage app passwords with hello MyApps portal](#manage-app-passwords-with-the-myapps-portal) section of this article.</span></span> 


## <a name="manage-app-passwords-in-hello-azure-portal"></a><span data-ttu-id="30c85-148">Správa hesel aplikací v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="30c85-148">Manage app passwords in hello Azure portal</span></span>
<span data-ttu-id="30c85-149">Pokud používáte dvoustupňové ověřování s Azure, budete chtít toocreate hesla aplikací prostřednictvím hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="30c85-149">If you use two-step verification with Azure, you want toocreate app passwords through hello Azure portal.</span></span>

### <a name="toocreate-app-passwords-in-hello-azure-portal"></a><span data-ttu-id="30c85-150">hesla aplikací toocreate v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="30c85-150">toocreate app passwords in hello Azure portal</span></span>
1. <span data-ttu-id="30c85-151">Přihlaste se toohello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="30c85-151">Sign in toohello Azure classic portal.</span></span>
2. <span data-ttu-id="30c85-152">V horní části hello klikněte pravým tlačítkem na své uživatelské jméno a vyberte další ověření zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="30c85-152">At hello top, right-click your user name and select Additional Security Verification.</span></span>
3. <span data-ttu-id="30c85-153">Na stránce ověření hello hello horní vyberte hesel aplikací</span><span class="sxs-lookup"><span data-stu-id="30c85-153">On hello proofup page, at hello top, select app passwords</span></span>
4. <span data-ttu-id="30c85-154">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="30c85-154">Click **Create**.</span></span>
5. <span data-ttu-id="30c85-155">Zadejte název hesla aplikace hello a klikněte na tlačítko **další**</span><span class="sxs-lookup"><span data-stu-id="30c85-155">Enter a name for hello app password and click **Next**</span></span>
6. <span data-ttu-id="30c85-156">Hello aplikace heslo toohello schránky zkopírujte a vložte jej do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="30c85-156">Copy hello app password toohello clipboard and paste it into your app.</span></span>
   
   ![Cloud](./media/multi-factor-authentication-end-user-app-passwords/app2.png)


### <a name="toodelete-app-passwords-in-hello-azure-portal"></a><span data-ttu-id="30c85-158">hesla aplikací toodelete v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="30c85-158">toodelete app passwords in hello Azure portal</span></span>
1. <span data-ttu-id="30c85-159">Přihlaste se toohello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="30c85-159">Sign in toohello Azure classic portal.</span></span>
2. <span data-ttu-id="30c85-160">V horní části hello klikněte pravým tlačítkem na své uživatelské jméno a vyberte další ověření zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="30c85-160">At hello top, right-click your user name and select Additional Security Verification.</span></span>
3. <span data-ttu-id="30c85-161">V horní části hello, další ověření zabezpečení tooadditional, vyberte **hesla aplikací.**</span><span class="sxs-lookup"><span data-stu-id="30c85-161">At hello top, next tooadditional security verification, select **app passwords.**</span></span>
4. <span data-ttu-id="30c85-162">Chcete toodelete, vyberte další heslo aplikace toohello **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="30c85-162">Next toohello app password you want toodelete, select **Delete**.</span></span>
5. <span data-ttu-id="30c85-163">Potvrdit odstranění hello kliknutím **Ano**.</span><span class="sxs-lookup"><span data-stu-id="30c85-163">Confirm hello deletion by clicking **yes**.</span></span>
6. <span data-ttu-id="30c85-164">Po odstranění hesla aplikace hello můžete kliknout na **zavřete**.</span><span class="sxs-lookup"><span data-stu-id="30c85-164">Once hello app password is deleted, you can click **close**.</span></span>


## <a name="manage-app-passwords-with-hello-myapps-portal"></a><span data-ttu-id="30c85-165">Spravujte hesla aplikací s hello MyApps portálu.</span><span class="sxs-lookup"><span data-stu-id="30c85-165">Manage app passwords with hello MyApps portal.</span></span>
<span data-ttu-id="30c85-166">Pokud si nejste jisti, jak používat ověřování Multi-Factor authentication, pak můžete vždy vytvořit a odstranit hesla aplikací prostřednictvím portálu myapps hello.</span><span class="sxs-lookup"><span data-stu-id="30c85-166">If you are not sure how you use multi-factor authentication, then you can always create and delete app passwords through hello myapps portal.</span></span>

### <a name="toocreate-an-app-password-using-hello-myapps-portal"></a><span data-ttu-id="30c85-167">toocreate heslo aplikace pomocí hello Myapps portálu</span><span class="sxs-lookup"><span data-stu-id="30c85-167">toocreate an app password using hello Myapps portal</span></span>
1. <span data-ttu-id="30c85-168">Přihlaste se příliš[https://myapps.microsoft.com](https://myapps.microsoft.com)</span><span class="sxs-lookup"><span data-stu-id="30c85-168">Sign in too[https://myapps.microsoft.com](https://myapps.microsoft.com)</span></span>
2. <span data-ttu-id="30c85-169">Klikněte na název v pravé horní části hello a zvolte **profil**.</span><span class="sxs-lookup"><span data-stu-id="30c85-169">Click your name at hello top right, and choose **Profile**.</span></span>
3. <span data-ttu-id="30c85-170">Vyberte **dalšího ověření zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="30c85-170">Select **Additional Security Verification**.</span></span>
   <span data-ttu-id="30c85-171">![Vyberte další bezpečnostní ověření – snímek obrazovky](./media/multi-factor-authentication-end-user-manage/myapps1.png)</span><span class="sxs-lookup"><span data-stu-id="30c85-171">![Select additional security verification - screenshot](./media/multi-factor-authentication-end-user-manage/myapps1.png)</span></span>

4. <span data-ttu-id="30c85-172">Vyberte **hesla aplikací**.</span><span class="sxs-lookup"><span data-stu-id="30c85-172">Select **app passwords**.</span></span>
   <span data-ttu-id="30c85-173">![Vyberte hesel aplikací – snímek obrazovky](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)</span><span class="sxs-lookup"><span data-stu-id="30c85-173">![Select app passwords - screenshot](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)</span></span>

5. <span data-ttu-id="30c85-174">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="30c85-174">Click **Create**.</span></span>
6. <span data-ttu-id="30c85-175">Zadejte název hesla aplikace hello a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="30c85-175">Enter a name for hello app password and click **Next**.</span></span>
7. <span data-ttu-id="30c85-176">Hello aplikace heslo toohello schránky zkopírujte a vložte jej do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="30c85-176">Copy hello app password toohello clipboard and paste it into your app.</span></span>
   <span data-ttu-id="30c85-177">![Vytvořit heslo aplikace](./media/multi-factor-authentication-end-user-app-passwords/create2.png)</span><span class="sxs-lookup"><span data-stu-id="30c85-177">![Create an app password](./media/multi-factor-authentication-end-user-app-passwords/create2.png)</span></span>

### <a name="toodelete-an-app-password-using-hello-myapps-portal"></a><span data-ttu-id="30c85-178">toodelete heslo aplikace pomocí hello Myapps portálu</span><span class="sxs-lookup"><span data-stu-id="30c85-178">toodelete an app password using hello Myapps portal</span></span>
1. <span data-ttu-id="30c85-179">Přihlaste se příliš[https://myapps.microsoft.com](https://myapps.microsoft.com)</span><span class="sxs-lookup"><span data-stu-id="30c85-179">Sign in too[https://myapps.microsoft.com](https://myapps.microsoft.com)</span></span>
2. <span data-ttu-id="30c85-180">V horní části hello vyberte profil.</span><span class="sxs-lookup"><span data-stu-id="30c85-180">At hello top, select profile.</span></span>
3. <span data-ttu-id="30c85-181">Vyberte **dalšího ověření zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="30c85-181">Select **Additional Security Verification**.</span></span>

   ![Vyberte další bezpečnostní ověření – snímek obrazovky](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. <span data-ttu-id="30c85-183">Vyberte **hesla aplikací**.</span><span class="sxs-lookup"><span data-stu-id="30c85-183">Select **app passwords**.</span></span>

   ![Vyberte hesel aplikací – snímek obrazovky](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. <span data-ttu-id="30c85-185">Klikněte na tlačítko Další heslo aplikace toohello chcete toodelete, **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="30c85-185">Next toohello app password you want toodelete, click **Delete**.</span></span>

   ![Odstranit heslo aplikace](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

6. <span data-ttu-id="30c85-187">Potvrďte, že chcete toodelete toto heslo kliknutím **Ano**.</span><span class="sxs-lookup"><span data-stu-id="30c85-187">Confirm that you want toodelete that password by clicking **yes**.</span></span>
7. <span data-ttu-id="30c85-188">Po odstranění hesla aplikace hello můžete kliknout na **zavřete**.</span><span class="sxs-lookup"><span data-stu-id="30c85-188">Once hello app password is deleted, you can click **close**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="30c85-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="30c85-189">Next steps</span></span>

- [<span data-ttu-id="30c85-190">Spravovat nastavení dvoustupňového ověřování</span><span class="sxs-lookup"><span data-stu-id="30c85-190">Manage your two-step verification settings</span></span>](multi-factor-authentication-end-user-manage-settings.md)

- <span data-ttu-id="30c85-191">Vyzkoušet hello [aplikaci Microsoft Authenticator](microsoft-authenticator-app-how-to.md) tooverify vaše přihlášení s oznámeními aplikace místo přijetí texty nebo volání.</span><span class="sxs-lookup"><span data-stu-id="30c85-191">Try out hello [Microsoft Authenticator app](microsoft-authenticator-app-how-to.md) tooverify your sign-ins with app notifications, instead of receiving texts or calls.</span></span> 
