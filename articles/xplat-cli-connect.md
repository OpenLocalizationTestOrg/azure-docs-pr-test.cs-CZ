---
title: "aaaLog v tooAzure z hello rozhraní příkazového řádku | Microsoft Docs"
description: "Připojit tooyour předplatné Azure pro Mac, Linux a Windows z hello rozhraní příkazového řádku Azure (Azure CLI)"
editor: tysonn
manager: timlt
documentationcenter: 
author: squillace
services: virtual-machines-linux,virtual-network,storage,azure-resource-manager
tags: azure-resource-manager,azure-service-management
ms.assetid: ed856527-d75e-4e16-93fb-253dafad209d
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: rasquill
"\"/": 
ms.openlocfilehash: 42682c00c8dea78b2c624e640379716d1d4d7a2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="log-in-tooazure-from-hello-azure-cli"></a><span data-ttu-id="18b2a-103">Přihlaste se tooAzure z hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="18b2a-103">Log in tooAzure from hello Azure CLI</span></span>
<span data-ttu-id="18b2a-104">Hello rozhraní příkazového řádku Azure je sada příkazů open source, a platformy pro práci s prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="18b2a-104">hello Azure CLI is a set of open-source, cross-platform commands for working with Azure resources.</span></span> <span data-ttu-id="18b2a-105">Tento článek popisuje různé způsoby tooprovide hello účet Azure pověření tooconnect hello rozhraní příkazového řádku Azure tooyour předplatné Azure:</span><span class="sxs-lookup"><span data-stu-id="18b2a-105">This article describes hello different ways tooprovide your Azure account credentials tooconnect hello Azure CLI tooyour Azure subscription:</span></span>

* <span data-ttu-id="18b2a-106">Spustit hello `azure login` rozhraní příkazového řádku příkaz tooauthenticate prostřednictvím Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="18b2a-106">Run hello `azure login` CLI command tooauthenticate through Azure Active Directory.</span></span> <span data-ttu-id="18b2a-107">Tato metoda poskytuje přístup tooCLI příkazy v obou [příkaz režimy](#cli-command-modes).</span><span class="sxs-lookup"><span data-stu-id="18b2a-107">This method gives you access tooCLI commands in both [command modes](#cli-command-modes).</span></span> <span data-ttu-id="18b2a-108">Když spustíte příkaz hello bez dalších možností, `azure login` vyzve vás toocontinue interaktivní přihlašování prostřednictvím webového portálu.</span><span class="sxs-lookup"><span data-stu-id="18b2a-108">When you run hello command without additional options, `azure login` prompts you toocontinue logging in interactively through a web portal.</span></span> <span data-ttu-id="18b2a-109">Pro další `azure login` příkaz Možnosti najdete v tématu hello scénáře v tomto článku nebo typ `azure login --help`.</span><span class="sxs-lookup"><span data-stu-id="18b2a-109">For additional `azure login` command options, see hello scenarios in this article, or type `azure login --help`.</span></span>
* <span data-ttu-id="18b2a-110">Pokud potřebujete jenom toouse Azure Service Management režimu rozhraní příkazového řádku (nedoporučuje se většina nových nasazení), můžete stáhnout a nainstalovat soubor nastavení publikování ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="18b2a-110">If you only need toouse Azure Service Management mode CLI commands (not recommended for most new deployments), you can download and install a publish settings file on your computer.</span></span>

<span data-ttu-id="18b2a-111">Pokud jste ještě nenainstalovali hello CLI, přečtěte si téma [hello instalace rozhraní příkazového řádku Azure](cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="18b2a-111">If you haven't already installed hello CLI, see [Install hello Azure CLI](cli-install-nodejs.md).</span></span> <span data-ttu-id="18b2a-112">Pokud nemáte předplatné Azure, můžete si [bezplatně vytvořit účet](http://azure.microsoft.com/free/) během několika minut.</span><span class="sxs-lookup"><span data-stu-id="18b2a-112">If you don't have an Azure subscription, you can create a [free account](http://azure.microsoft.com/free/) in just a couple of minutes.</span></span>

<span data-ttu-id="18b2a-113">Informace o jiný účet identity a předplatná Azure, najdete v části [asociování předplatných Azure se službou Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md).</span><span class="sxs-lookup"><span data-stu-id="18b2a-113">For background about different account identities and Azure subscriptions, see [How Azure subscriptions are associated with Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md).</span></span>

## <a name="scenario-1-azure-login-with-interactive-login"></a><span data-ttu-id="18b2a-114">Scénář 1: přihlášení k azure s interaktivní přihlášení</span><span class="sxs-lookup"><span data-stu-id="18b2a-114">Scenario 1: azure login with interactive login</span></span>
<span data-ttu-id="18b2a-115">S určitým účtům hello rozhraní příkazového řádku, musíte toorun `azure login` a pokračujte v procesu přihlášení hello s webovým prohlížečem, přes webový portál, procesu označovaného jako *interaktivní přihlášení*.</span><span class="sxs-lookup"><span data-stu-id="18b2a-115">With certain accounts, hello CLI requires you toorun `azure login` and then continue hello login process with a web browser through a web portal, a process called *interactive login*.</span></span> <span data-ttu-id="18b2a-116">Obvyklým důvodem je, když máte pracovní nebo školní účet (také nazývané *účet organizace*) je nastavený toorequire vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="18b2a-116">A common reason is when you have a work or school account (also called an *organizational account*) that is set up toorequire multifactor authentication.</span></span> <span data-ttu-id="18b2a-117">Pokud chcete příkazy v režimu Resource Manager toouse také používejte interaktivní přihlášení pomocí účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="18b2a-117">Also use interactive login with your Microsoft account, when you want toouse Resource Manager mode commands.</span></span>

<span data-ttu-id="18b2a-118">Interaktivní přihlášení je snadné: typ `azure login` – bez jakékoli možnosti – jak ukazuje následující příklad hello:</span><span class="sxs-lookup"><span data-stu-id="18b2a-118">Interactive login is easy: type `azure login` -- without any options -- as shown in hello following example:</span></span>

```
azure login
```                                                                                             

<span data-ttu-id="18b2a-119">Zobrazí se výstup Hello něco podobného jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="18b2a-119">hello output appears something like hello following:</span></span>

```         
info:    Executing command login
info:    toosign in, use a web browser tooopen hello page http://aka.ms/devicelogin. Enter hello code XXXXXXXXX tooauthenticate.
```
<span data-ttu-id="18b2a-120">Zkopírujte kód hello nabízí tooyou ve výstupu příkazu hello a otevřete prohlížeč toohttp://aka.ms/devicelogin nebo jinou stránku, pokud zadaný.</span><span class="sxs-lookup"><span data-stu-id="18b2a-120">Copy hello code offered tooyou in hello command output, and open a browser toohttp://aka.ms/devicelogin, or other page if specified.</span></span> <span data-ttu-id="18b2a-121">(Můžete otevřít prohlížeč na hello stejný počítač, nebo na jiný počítač nebo zařízení.) Zadejte kód hello a pak jste výzvami tooenter hello uživatelské jméno a heslo pro identitu hello chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="18b2a-121">(You can open a browser on hello same computer, or on a different computer or device.) Enter hello code, and then you are prompted tooenter hello username and password for hello identity you want toouse.</span></span> <span data-ttu-id="18b2a-122">Po dokončení tohoto procesu hello příkazové okno dokončení přihlášení hello.</span><span class="sxs-lookup"><span data-stu-id="18b2a-122">When that process completes, hello command shell completes hello login.</span></span> <span data-ttu-id="18b2a-123">Ho může vypadat podobně jako:</span><span class="sxs-lookup"><span data-stu-id="18b2a-123">It might look something like:</span></span>

    info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Added subscription Azure Free Trial
    info:    Setting subscription "Visual Studio Ultimate with MSDN" as default
    +
    info:    login command OK

> [!NOTE]
> <span data-ttu-id="18b2a-124">S interaktivní přihlášení ověřování a autorizace se provádí pomocí služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="18b2a-124">With interactive login, authentication and authorization are performed using Azure Active Directory.</span></span> <span data-ttu-id="18b2a-125">Pokud chcete použít identitu účtu Microsoft, přistupuje k procesu přihlášení hello výchozí doménu služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="18b2a-125">If you use a Microsoft account identity, hello login process accesses your Azure Active Directory default domain.</span></span> <span data-ttu-id="18b2a-126">(Pokud jste zaregistrovali bezplatný účet Azure, Azure Active Directory automaticky vytvoří výchozí doménu pro účet.)</span><span class="sxs-lookup"><span data-stu-id="18b2a-126">(If you signed up for a free Azure account, Azure Active Directory automatically created a default domain for your account.)</span></span>
>
>

## <a name="scenario-2-azure-login-with-a-username-and-password"></a><span data-ttu-id="18b2a-127">Scénář 2: azure přihlášení pomocí uživatelského jména a hesla</span><span class="sxs-lookup"><span data-stu-id="18b2a-127">Scenario 2: azure login with a username and password</span></span>
<span data-ttu-id="18b2a-128">Použití hello `azure login` příkazu s uživatelským jménem hello (`-u`) tooauthenticate parametr, pokud chcete, aby toouse pracovní nebo školní účet, který nevyžaduje vícefaktorového ověřování.</span><span class="sxs-lookup"><span data-stu-id="18b2a-128">Use hello `azure login` command with hello username (`-u`) parameter tooauthenticate when you want toouse a work or school account that doesn't require multifactor authentication.</span></span> <span data-ttu-id="18b2a-129">Zobrazí se výzva v příkazovém řádku hello hello hesla (nebo můžete volitelně předat hello heslo jako dodatečný parametr hello `azure login` příkaz).</span><span class="sxs-lookup"><span data-stu-id="18b2a-129">You are prompted at hello command line for hello password (or you can optionally pass hello password as an additional parameter of hello `azure login` command).</span></span> <span data-ttu-id="18b2a-130">Hello následující příklad předá hello uživatelské jméno účtu organizace:</span><span class="sxs-lookup"><span data-stu-id="18b2a-130">hello following example passes hello username of an organizational account:</span></span>

    azure login -u myUserName@contoso.onmicrosoft.com

<span data-ttu-id="18b2a-131">Pak se zobrazí výzva tooenter heslo:</span><span class="sxs-lookup"><span data-stu-id="18b2a-131">You are then prompted tooenter your password:</span></span>

    info:    Executing command login
    Password: *********

<span data-ttu-id="18b2a-132">potom dokončí proces přihlášení Hello.</span><span class="sxs-lookup"><span data-stu-id="18b2a-132">hello login process then completes.</span></span>

    info:    Added subscription Visual Studio Ultimate with MSDN
    +
    info:    login command OK

<span data-ttu-id="18b2a-133">Pokud je vaše první protokolování v čase se pomocí těchto přihlašovacích údajů, se zobrazí výzva tooverify chcete toocache ověřovací token.</span><span class="sxs-lookup"><span data-stu-id="18b2a-133">If this is your first time logging in with these credentials, you are asked tooverify that you wish toocache an authentication token.</span></span> <span data-ttu-id="18b2a-134">Tato výzva také nastat, pokud jste dříve používali hello `azure logout` příkazu (popsaný v článku hello později).</span><span class="sxs-lookup"><span data-stu-id="18b2a-134">This prompt also occurs if you previously used hello `azure logout` command (described later in hello article).</span></span> <span data-ttu-id="18b2a-135">spustit tuto výzvu pro scénáře automatizace toobypass `azure login` s hello `-q` parametr.</span><span class="sxs-lookup"><span data-stu-id="18b2a-135">toobypass this prompt for automation scenarios, run `azure login` with hello `-q` parameter.</span></span>

## <a name="scenario-3-azure-login-with-a-service-principal"></a><span data-ttu-id="18b2a-136">Scénář 3: přihlášení k azure pomocí objektu služby</span><span class="sxs-lookup"><span data-stu-id="18b2a-136">Scenario 3: azure login with a service principal</span></span>
<span data-ttu-id="18b2a-137">Pokud vytvoříte objekt služby pro aplikaci služby Active Directory a objektu služby hello má oprávnění pro vaše předplatné, můžete použít hello `azure login` příkaz tooauthenticate hello instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="18b2a-137">If you create a service principal for an Active Directory application, and hello service principal has permissions on your subscription, you can use hello `azure login` command tooauthenticate hello service principal.</span></span> <span data-ttu-id="18b2a-138">V závislosti na scénáři, je možné zadat přihlašovací údaje hello hello instanční objekt jako explicitní parametry hello `azure login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="18b2a-138">Depending on your scenario, you could provide hello credentials of hello service principal as explicit parameters of hello `azure login` command.</span></span> <span data-ttu-id="18b2a-139">Například hello následující příkaz předává hello hlavní název služby a služby Active Directory ID klienta:</span><span class="sxs-lookup"><span data-stu-id="18b2a-139">For example, hello following command passes hello service principal name and Active Directory tenant ID:</span></span>

    azure login -u https://www.contoso.org/example --service-principal --tenant myTenantID

<span data-ttu-id="18b2a-140">Můžete se pak výzvami tooprovide hello heslo.</span><span class="sxs-lookup"><span data-stu-id="18b2a-140">You are then prompted tooprovide hello password.</span></span> <span data-ttu-id="18b2a-141">Můžete také zadat přihlašovací údaje hello prostřednictvím rozhraní příkazového řádku skriptu nebo aplikace kódu nebo použít objekt certifikátu tooauthenticate hello služby neinteraktivně pro scénáře automatizace.</span><span class="sxs-lookup"><span data-stu-id="18b2a-141">You can also provide hello credentials through a CLI script or application code, or use a certificate tooauthenticate hello service principal non-interactively for automation scenarios.</span></span> <span data-ttu-id="18b2a-142">Podrobnosti a příklady naleznete v tématu [ověřování hlavní název služby pomocí Azure Resource Manageru](resource-group-authenticate-service-principal-cli.md).</span><span class="sxs-lookup"><span data-stu-id="18b2a-142">For details and examples, see [Authenticating a service principal with Azure Resource Manager](resource-group-authenticate-service-principal-cli.md).</span></span>

## <a name="scenario-4-use-a-publish-settings-file"></a><span data-ttu-id="18b2a-143">Scénář 4: Použijte soubor nastavení publikování</span><span class="sxs-lookup"><span data-stu-id="18b2a-143">Scenario 4: Use a publish settings file</span></span>
<span data-ttu-id="18b2a-144">Pokud potřebujete jenom toouse hello Azure Service Management režimu rozhraní příkazového řádku (například toodeploy virtuální počítače Azure v modelu nasazení classic hello), se můžete připojit pomocí soubor nastavení publikování.</span><span class="sxs-lookup"><span data-stu-id="18b2a-144">If you only need toouse hello Azure Service Management mode CLI commands (for example, toodeploy Azure VMs in hello classic deployment model), you can connect using a publish settings file.</span></span> <span data-ttu-id="18b2a-145">Tato metoda nainstaluje certifikát v místním počítači, který umožňuje tooperform úlohy správy pro také hello předplatném a certifikátu hello jsou platné.</span><span class="sxs-lookup"><span data-stu-id="18b2a-145">This method installs a certificate on your local computer that allows you tooperform management tasks for as long as hello subscription and hello certificate are valid.</span></span>

* <span data-ttu-id="18b2a-146">**soubor nastavení publikování toodownload hello** pro váš účet, ujistěte se, že hello rozhraní příkazového řádku je v režimu správy služby zadáním `azure config mode asm`.</span><span class="sxs-lookup"><span data-stu-id="18b2a-146">**toodownload hello publish settings file** for your account, ensure that hello CLI is in Service Management mode by typing `azure config mode asm`.</span></span> <span data-ttu-id="18b2a-147">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="18b2a-147">Then run hello following command:</span></span>

        azure account download

<span data-ttu-id="18b2a-148">To otevře výchozí prohlížeč a zobrazí výzvu toosign v toohello [portál Azure classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="18b2a-148">This opens your default browser and prompts you toosign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="18b2a-149">Po přihlášení, `.publishsettings` stahování souborů.</span><span class="sxs-lookup"><span data-stu-id="18b2a-149">After you sign in, a `.publishsettings` file downloads.</span></span> <span data-ttu-id="18b2a-150">Poznamenejte si, kam se soubor uloží.</span><span class="sxs-lookup"><span data-stu-id="18b2a-150">Make note of where this file is saved.</span></span>

> [!NOTE]
> <span data-ttu-id="18b2a-151">Pokud váš účet je spojen s více klienty Azure Active Directory, může být výzvami tooselect, který chcete toodownload nastavení publikování služby Active Directory v souboru.</span><span class="sxs-lookup"><span data-stu-id="18b2a-151">If your account is associated with multiple Azure Active Directory tenants, you may be prompted tooselect which Active Directory you wish toodownload a publish settings file for.</span></span>
>
>

<span data-ttu-id="18b2a-152">Po výběru pomocí stránky pro stažení hello nebo návštěvou hello portál Azure classic, hello vybrané služby Active Directory změní výchozí hello používá hello klasický portál a stránky pro stažení.</span><span class="sxs-lookup"><span data-stu-id="18b2a-152">Once selected using hello download page, or by visiting hello Azure classic portal, hello selected Active Directory becomes hello default used by hello classic portal and download page.</span></span> <span data-ttu-id="18b2a-153">Po vytvoření výchozí, zobrazí se hello text,**kliknutím sem stránka pro výběr toohello tooreturn**' hello horní části stránky pro stažení hello.</span><span class="sxs-lookup"><span data-stu-id="18b2a-153">Once a default has been established, you see hello text '**click here tooreturn toohello selection page**' at hello top of hello download page.</span></span> <span data-ttu-id="18b2a-154">Použití hello zadat odkaz tooreturn toohello výběr stránky.</span><span class="sxs-lookup"><span data-stu-id="18b2a-154">Use hello provided link tooreturn toohello selection page.</span></span>

* <span data-ttu-id="18b2a-155">**soubor nastavení publikování tooimport hello**spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="18b2a-155">**tooimport hello publish settings file**, run hello following command:</span></span>

        azure account import <path tooyour .publishsettings file>

> [!IMPORTANT]
> <span data-ttu-id="18b2a-156">Po importu vaše nastavení publikování, měli byste odstranit hello `.publishsettings` souboru.</span><span class="sxs-lookup"><span data-stu-id="18b2a-156">After importing your publish settings, you should delete hello `.publishsettings` file.</span></span> <span data-ttu-id="18b2a-157">Je už nevyžadovala hello rozhraní příkazového řádku Azure a představuje bezpečnostní riziko, může se použít toogain přístup tooyour předplatné.</span><span class="sxs-lookup"><span data-stu-id="18b2a-157">It is no longer required by hello Azure CLI and presents a security risk as it could be used toogain access tooyour subscription.</span></span>
>
>

## <a name="cli-command-modes"></a><span data-ttu-id="18b2a-158">Režimy rozhraní příkazového řádku příkaz</span><span class="sxs-lookup"><span data-stu-id="18b2a-158">CLI command modes</span></span>
<span data-ttu-id="18b2a-159">Hello rozhraní příkazového řádku Azure obsahuje dva režimy příkaz pro práci s prostředky Azure, se sadami jiný příkaz:</span><span class="sxs-lookup"><span data-stu-id="18b2a-159">hello Azure CLI provides two command modes for working with Azure resources, with different command sets:</span></span>

* <span data-ttu-id="18b2a-160">**Režimu Resource Manager** – pro práci s prostředky Azure v modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="18b2a-160">**Resource Manager mode** - for working with Azure resources in hello Resource Manager deployment model.</span></span> <span data-ttu-id="18b2a-161">tooset tento režim spuštění `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="18b2a-161">tooset this mode, run `azure config mode arm`.</span></span>
* <span data-ttu-id="18b2a-162">**Režim správy služby** – pro práci s prostředky Azure v modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="18b2a-162">**Service Management mode** - for working with Azure resources in hello classic deployment model.</span></span> <span data-ttu-id="18b2a-163">tooset tento režim spuštění `azure config mode asm`.</span><span class="sxs-lookup"><span data-stu-id="18b2a-163">tooset this mode, run `azure config mode asm`.</span></span>

<span data-ttu-id="18b2a-164">Při první instalaci hello aktuální verzi hello rozhraní příkazového řádku je v režimu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="18b2a-164">When first installed, hello current release of hello CLI is in Resource Manager mode.</span></span>

> [!NOTE]
> <span data-ttu-id="18b2a-165">Hello režimu Resource Manager a režimu správy služby se vzájemně vylučují.</span><span class="sxs-lookup"><span data-stu-id="18b2a-165">hello Resource Manager mode and Service Management mode are mutually exclusive.</span></span> <span data-ttu-id="18b2a-166">To znamená, se nedají spravovat prostředky vytvořené v jednom režimu z hello jiných režimu.</span><span class="sxs-lookup"><span data-stu-id="18b2a-166">That is, resources created in one mode cannot be managed from hello other mode.</span></span>
>
>

## <a name="multiple-subscriptions"></a><span data-ttu-id="18b2a-167">Více předplatných</span><span class="sxs-lookup"><span data-stu-id="18b2a-167">Multiple subscriptions</span></span>
<span data-ttu-id="18b2a-168">Pokud máte víc předplatných Azure, připojení tooAzure uděluje přístup tooall předplatná spojená s přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="18b2a-168">If you have multiple Azure subscriptions, connecting tooAzure grants access tooall subscriptions associated with your credentials.</span></span> <span data-ttu-id="18b2a-169">Jedno předplatné, je vybrána jako výchozí hello a použít hello rozhraní příkazového řádku Azure při provádění operací.</span><span class="sxs-lookup"><span data-stu-id="18b2a-169">One subscription is selected as hello default, and used by hello Azure CLI when performing operations.</span></span> <span data-ttu-id="18b2a-170">Můžete zobrazit hello předplatného, včetně hello aktuální výchozí předplatné, pomocí hello `azure account list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="18b2a-170">You can view hello subscriptions, including hello current default subscription, using hello `azure account list` command.</span></span> <span data-ttu-id="18b2a-171">Tento příkaz vrátí podobné toohello následující informace:</span><span class="sxs-lookup"><span data-stu-id="18b2a-171">This command returns information similar toohello following:</span></span>

    info:    Executing command account list
    data:    Name              Id                                    Current
    data:    ----------------  ------------------------------------  -------
    data:    Azure-sub-1       ####################################  true
    data:    Azure-sub-2       ####################################  false

<span data-ttu-id="18b2a-172">V předchozím seznamu hello, hello **aktuální** sloupec zobrazuje informace o hello aktuální výchozí předplatné jako Azure-sub-1.</span><span class="sxs-lookup"><span data-stu-id="18b2a-172">In hello preceding list, hello **Current** column indicates hello current default subscription as Azure-sub-1.</span></span> <span data-ttu-id="18b2a-173">toochange hello výchozí předplatné, použijte hello `azure account set` příkaz a zadejte hello předplatné chcete toobe hello výchozí.</span><span class="sxs-lookup"><span data-stu-id="18b2a-173">toochange hello default subscription, use hello `azure account set` command, and specify hello subscription that you wish toobe hello default.</span></span> <span data-ttu-id="18b2a-174">Například:</span><span class="sxs-lookup"><span data-stu-id="18b2a-174">For example:</span></span>

    azure account set Azure-sub-2

<span data-ttu-id="18b2a-175">Tato operace změní hello výchozí předplatné tooAzure-sub-2.</span><span class="sxs-lookup"><span data-stu-id="18b2a-175">This changes hello default subscription tooAzure-sub-2.</span></span>

> [!NOTE]
> <span data-ttu-id="18b2a-176">Změna výchozí předplatné hello se okamžitě projeví a globální změně; nové příkazy rozhraní příkazového řádku Azure, zda je spouštět z hello stejné instance příkazového řádku nebo jinou instanci, použijte nové předplatné výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="18b2a-176">Changing hello default subscription takes effect immediately, and is a global change; new Azure CLI commands, whether you run them from hello same command-line instance or a different instance, use hello new default subscription.</span></span>
>
>

<span data-ttu-id="18b2a-177">Pokud chcete toouse jiné než výchozí předplatné s hello rozhraní příkazového řádku Azure, ale nechcete, aby toochange hello aktuální výchozí nastavení, můžete použít hello `--subscription` možnost pro příkaz hello a zadejte název hello hello předplatného chcete toouse pro operaci hello.</span><span class="sxs-lookup"><span data-stu-id="18b2a-177">If you wish toouse a non-default subscription with hello Azure CLI, but don't want toochange hello current default, you can use hello `--subscription` option for hello command and provide hello name of hello subscription you wish toouse for hello operation.</span></span>

<span data-ttu-id="18b2a-178">Až se připojené tooyour předplatné Azure, můžete začít používat toowork příkazy příkazového řádku Azure CLI hello s prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="18b2a-178">Once you are connected tooyour Azure subscription, you can start using hello Azure CLI commands toowork with Azure resources.</span></span>

## <a name="storage-of-cli-settings"></a><span data-ttu-id="18b2a-179">Ukládání nastavení rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="18b2a-179">Storage of CLI settings</span></span>
<span data-ttu-id="18b2a-180">Jestli přihlásit hello `azure login` příkaz nebo importu nastavení publikování, profil rozhraní příkazového řádku a protokoly jsou uloženy v `.azure` adresář umístěný ve vaší `user` adresáře.</span><span class="sxs-lookup"><span data-stu-id="18b2a-180">Whether you log in with hello `azure login` command or import publish settings, your CLI profile and logs are stored in a `.azure` directory located in your `user` directory.</span></span> <span data-ttu-id="18b2a-181">Vaše `user` adresář je chráněn váš operační systém.</span><span class="sxs-lookup"><span data-stu-id="18b2a-181">Your `user` directory is protected by your operating system.</span></span> <span data-ttu-id="18b2a-182">Doporučujeme však, že je provést další kroky tooencrypt vaše `user` adresáře.</span><span class="sxs-lookup"><span data-stu-id="18b2a-182">However, we recommend that you take additional steps tooencrypt your `user` directory.</span></span> <span data-ttu-id="18b2a-183">Můžete tak učinit v hello následující způsoby:</span><span class="sxs-lookup"><span data-stu-id="18b2a-183">You can do so in hello following ways:</span></span>

* <span data-ttu-id="18b2a-184">V systému Windows upravit vlastnosti directory hello nebo pomocí nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="18b2a-184">On Windows, modify hello directory properties or use BitLocker.</span></span>
* <span data-ttu-id="18b2a-185">V systému Mac zapněte pro adresář hello FileVault.</span><span class="sxs-lookup"><span data-stu-id="18b2a-185">On Mac, turn on FileVault for hello directory.</span></span>
* <span data-ttu-id="18b2a-186">Na Ubuntu použijte hello šifrovaný domovské adresáře funkce.</span><span class="sxs-lookup"><span data-stu-id="18b2a-186">On Ubuntu, use hello Encrypted Home directory feature.</span></span> <span data-ttu-id="18b2a-187">Další Linuxových distribucích nabízí podobné funkce.</span><span class="sxs-lookup"><span data-stu-id="18b2a-187">Other Linux distributions offer similar features.</span></span>

## <a name="logging-out"></a><span data-ttu-id="18b2a-188">Protokolování se</span><span class="sxs-lookup"><span data-stu-id="18b2a-188">Logging out</span></span>
<span data-ttu-id="18b2a-189">toolog out hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="18b2a-189">toolog out, use hello following command:</span></span>

    azure logout -u <username>

<span data-ttu-id="18b2a-190">Pokud hello odběry přidružený účet hello pouze ověření službou Active Directory, protokolování se odstraní informace o předplatném hello z místního profilu hello.</span><span class="sxs-lookup"><span data-stu-id="18b2a-190">If hello subscriptions associated with hello account are only authenticated with Active Directory, logging out deletes hello subscription information from hello local profile.</span></span> <span data-ttu-id="18b2a-191">Ale pokud soubor nastavení publikování byl importován také odběrů hello, protokolování jen odstranění služby Active Directory související informace z profilu místní hello.</span><span class="sxs-lookup"><span data-stu-id="18b2a-191">However, if a publish settings file was also imported for hello subscriptions, logging out only deletes Active Directory related information from hello local profile.</span></span>

## <a name="next-steps"></a><span data-ttu-id="18b2a-192">Další kroky</span><span class="sxs-lookup"><span data-stu-id="18b2a-192">Next steps</span></span>
* <span data-ttu-id="18b2a-193">toouse rozhraní příkazového řádku Azure, najdete v části [rozhraní příkazového řádku Azure v režimu Resource Manager](virtual-machines/azure-cli-arm-commands.md) a [rozhraní příkazového řádku Azure v režimu správy služby](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="18b2a-193">toouse Azure CLI commands, see [Azure CLI commands in Resource Manager mode](virtual-machines/azure-cli-arm-commands.md) and [Azure CLI commands in Service Management mode](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>
* <span data-ttu-id="18b2a-194">toolearn Další informace o hello rozhraní příkazového řádku Azure, zdrojový kód, odesílat zprávy o problémech, nebo přispívat toohello projekt stáhnete na adrese hello [úložiště GitHub pro hello rozhraní příkazového řádku Azure](https://github.com/azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="18b2a-194">toolearn more about hello Azure CLI, download source code, report problems, or contribute toohello project, visit hello [GitHub repository for hello Azure CLI](https://github.com/azure/azure-xplat-cli).</span></span>
* <span data-ttu-id="18b2a-195">Pokud narazíte na problémy pomocí hello rozhraní příkazového řádku Azure nebo Azure, navštivte hello [fóra Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span><span class="sxs-lookup"><span data-stu-id="18b2a-195">If you encounter problems using hello Azure CLI, or Azure, visit hello [Azure Forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span></span>
