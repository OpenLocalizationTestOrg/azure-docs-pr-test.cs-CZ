---
title: "aaaUse prostředí PowerShell tooCreate virtuálních počítačů s nativní režim serveru sestav | Microsoft Docs"
description: "Toto téma popisuje a provede hello nasazení a konfiguraci serveru sestav v nativním režimu SQL Server Reporting Services ve virtuální počítač Azure. "
services: virtual-machines-windows
documentationcenter: na
author: guyinacube
manager: erikre
editor: monicar
tags: azure-service-management
ms.assetid: 553af55b-d02e-4e32-904c-682bfa20fa0f
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/11/2017
ms.author: asaxton
ms.openlocfilehash: e7791199c87dff106132f1535da12de40a8dbc9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toocreate-an-azure-vm-with-a-native-mode-report-server"></a><span data-ttu-id="ef6c1-103">Pomocí prostředí PowerShell tooCreate Azure virtuálních počítačů s nativní režim serveru sestav</span><span class="sxs-lookup"><span data-stu-id="ef6c1-103">Use PowerShell tooCreate an Azure VM With a Native Mode Report Server</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="ef6c1-104">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="ef6c1-105">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="ef6c1-106">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="ef6c1-107">Toto téma popisuje a provede hello nasazení a konfiguraci serveru sestav v nativním režimu SQL Server Reporting Services ve virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-107">This topic describes and walks you through hello deployment and configuration of a SQL Server Reporting Services native mode report server in an Azure Virtual Machine.</span></span> <span data-ttu-id="ef6c1-108">Hello kroky v tomto dokumentu kombinaci ruční kroky toocreate hello virtuálního počítače a skript prostředí Windows PowerShell tooconfigure Reporting Services na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-108">hello steps in this document use a combination of manual steps toocreate hello virtual machine and a Windows PowerShell script tooconfigure Reporting Services on hello VM.</span></span> <span data-ttu-id="ef6c1-109">Hello konfigurační skript zahrnuje otevřít port brány firewall pro protokol HTTP nebo HTTPs.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-109">hello configuration script includes opening a firewall port for HTTP or HTTPs.</span></span>

> [!NOTE]
> <span data-ttu-id="ef6c1-110">Pokud nechcete, aby **HTTPS** na server sestav hello **přeskočit krok 2**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-110">If you do not require **HTTPS** on hello report server, **skip step 2**.</span></span>
> 
> <span data-ttu-id="ef6c1-111">Po vytvoření hello virtuálního počítače v kroku 1, přejděte toohello části pomocí skriptu tooconfigure hello server sestav a HTTP.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-111">After creating hello VM in step 1, go toohello section Use script tooconfigure hello report server and HTTP.</span></span> <span data-ttu-id="ef6c1-112">Po spuštění skriptu hello hello serveru sestav je připraven toouse.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-112">After you run hello script, hello report server is ready toouse.</span></span>

## <a name="prerequisites-and-assumptions"></a><span data-ttu-id="ef6c1-113">Požadavky a předpoklady</span><span class="sxs-lookup"><span data-stu-id="ef6c1-113">Prerequisites and Assumptions</span></span>
* <span data-ttu-id="ef6c1-114">**Předplatné Azure**: Ověřte hello počet jader ve vašem předplatném Azure dostupná.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-114">**Azure Subscription**: Verify hello number of cores available in your Azure Subscription.</span></span> <span data-ttu-id="ef6c1-115">Pokud vytvoříte hello doporučená velikost virtuálního počítače u **A3**, je nutné **4** dostupné jader.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-115">If you create hello recommended VM size of **A3**, you need **4** available cores.</span></span> <span data-ttu-id="ef6c1-116">Pokud používáte velikost virtuálního počítače u **A2**, je nutné **2** dostupné jader.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-116">If you use a VM size of **A2**, you need **2** available cores.</span></span>
  
  * <span data-ttu-id="ef6c1-117">tooverify hello základní omezení předplatného v hello portál Azure classic, klikněte na tlačítko nastavení v levém podokně hello a pak klikněte na tlačítko využití v horní nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-117">tooverify hello core limit of your subscription, in hello Azure classic portal, click SETTINGS in hello left pane and then Click USAGE in hello top menu.</span></span>
  * <span data-ttu-id="ef6c1-118">tooincrease hello kvóta na jádra, kontaktujte [podporu Azure](https://azure.microsoft.com/support/options/).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-118">tooincrease hello core quota, contact [Azure Support](https://azure.microsoft.com/support/options/).</span></span> <span data-ttu-id="ef6c1-119">Informace o velikosti virtuálních počítačů, najdete v části [velikostí virtuálních počítačů pro Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-119">For VM size information, see [Virtual Machine Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="ef6c1-120">**Windows PowerShell skriptování**: hello tématu se předpokládá, že máte základní znalosti pracovní prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-120">**Windows PowerShell Scripting**: hello topic assumes that you have a basic working knowledge of Windows PowerShell.</span></span> <span data-ttu-id="ef6c1-121">Další informace o používání prostředí Windows PowerShell najdete v tématu hello následující:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-121">For more information about using Windows PowerShell, see hello following:</span></span>
  
  * [<span data-ttu-id="ef6c1-122">Spuštění prostředí Windows PowerShell v systému Windows Server</span><span class="sxs-lookup"><span data-stu-id="ef6c1-122">Starting Windows PowerShell on Windows Server</span></span>](https://technet.microsoft.com/library/hh847814.aspx)
  * [<span data-ttu-id="ef6c1-123">Začínáme s prostředím Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="ef6c1-123">Getting Started with Windows PowerShell</span></span>](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a><span data-ttu-id="ef6c1-124">Krok 1: Zřídit virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="ef6c1-124">Step 1: Provision an Azure Virtual Machine</span></span>
1. <span data-ttu-id="ef6c1-125">Procházejte toohello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-125">Browse toohello Azure classic portal.</span></span>
2. <span data-ttu-id="ef6c1-126">Klikněte na tlačítko **virtuální počítače** v levém podokně hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-126">Click **Virtual Machines** in hello left pane.</span></span>
   
    ![virtuální počítače Microsoft azure](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)
3. <span data-ttu-id="ef6c1-128">Klikněte na možnost **Nové**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-128">Click **New**.</span></span>
   
    ![tlačítko Nový](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)
4. <span data-ttu-id="ef6c1-130">Klikněte na tlačítko **z Galerie**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-130">Click **From Gallery**.</span></span>
   
    ![nový virtuální počítač z Galerie](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)
5. <span data-ttu-id="ef6c1-132">Klikněte na tlačítko **SQL Server 2014 RTM Standard – Windows Server 2012 R2** a pak klikněte na šipku toocontinue hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-132">Click **SQL Server 2014 RTM Standard – Windows Server 2012 R2** and then click hello arrow toocontinue.</span></span>
   
    ![Další](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
   
    <span data-ttu-id="ef6c1-134">Pokud potřebujete data služby Reporting Services hello řízené funkce předplatných, vyberte **SQL Server 2014 RTM Enterprise – Windows Server 2012 R2**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-134">If you need hello Reporting Services data driven subscriptions feature, choose **SQL Server 2014 RTM Enterprise – Windows Server 2012 R2**.</span></span> <span data-ttu-id="ef6c1-135">Další informace o edicích systému SQL Server a podporovaných funkcích najdete v tématu [funkce nepodporuje hello edice systému SQL Server 2012](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-135">For more information on SQL Server editions and feature support, see [Features Supported by hello Editions of SQL Server 2012](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).</span></span>
6. <span data-ttu-id="ef6c1-136">Na hello **konfigurace virtuálního počítače** upravte hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-136">On hello **Virtual machine configuration** page, edit hello following fields:</span></span>
   
   * <span data-ttu-id="ef6c1-137">Pokud je více než jedna **datum vydání verze**, vyberte hello nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-137">If there is more than one **VERSION RELEASE DATE**, select hello most recent version.</span></span>
   * <span data-ttu-id="ef6c1-138">**Název virtuálního počítače**: název počítače hello se používá na další stránce konfigurace hello jako název DNS služby Cloud výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-138">**Virtual Machine Name**: hello machine name is also used on hello next configuration page as hello default Cloud Service DNS name.</span></span> <span data-ttu-id="ef6c1-139">název DNS Hello musí být jedinečný mezi hello služby Azure.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-139">hello DNS name must be unique across hello Azure service.</span></span> <span data-ttu-id="ef6c1-140">Vezměte v úvahu konfigurace hello virtuálních počítačů s názvem počítače, který popisuje, jaké hello virtuálních počítačů se používá pro.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-140">Consider configuring hello VM with a computer name that describes what hello VM is used for.</span></span> <span data-ttu-id="ef6c1-141">Například ssrsnativecloud.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-141">For example ssrsnativecloud.</span></span>
   * <span data-ttu-id="ef6c1-142">**Úroveň**: standardní</span><span class="sxs-lookup"><span data-stu-id="ef6c1-142">**Tier**: Standard</span></span>
   * <span data-ttu-id="ef6c1-143">**Velikost: A3** je hello doporučená velikost virtuálního počítače pro úlohy SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-143">**Size:A3** is hello recommended VM size for SQL Server workloads.</span></span> <span data-ttu-id="ef6c1-144">Pokud virtuální počítač je použít jenom jako server sestav, je dostatečná velikost virtuálního počítače z A2, pokud server sestav hello vyskytne velké zatížení.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-144">If a VM is only used as a report server, a VM size of A2 is sufficient unless hello report server experiences a large workload.</span></span> <span data-ttu-id="ef6c1-145">Informace o cenách virtuálních počítačů, najdete v části [ceny služeb Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-145">For VM pricing information, see [Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/).</span></span>
   * <span data-ttu-id="ef6c1-146">**Nové uživatelské jméno**: zadáte název hello je vytvořen jako správce hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-146">**New User Name**: hello name you provide is created as an administrator on hello VM.</span></span>
   * <span data-ttu-id="ef6c1-147">**Nové heslo** a **potvrďte**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-147">**New Password** and **confirm**.</span></span> <span data-ttu-id="ef6c1-148">Toto heslo se používá pro nový účet správce hello a doporučuje se, že používáte silné heslo.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-148">This password is used for hello new administrator account and it is recommended you use a strong password.</span></span>
   * <span data-ttu-id="ef6c1-149">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-149">Click **Next**.</span></span> <span data-ttu-id="ef6c1-150">![Další](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)</span><span class="sxs-lookup"><span data-stu-id="ef6c1-150">![next](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)</span></span>
7. <span data-ttu-id="ef6c1-151">Na další stránku hello upravte hello následující pole:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-151">On hello next page, edit hello following fields:</span></span>
   
   * <span data-ttu-id="ef6c1-152">**Cloudová služba**: vyberte **vytvořte novou Cloudovou službu**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-152">**Cloud Service**: select **Create a new Cloud Service**.</span></span>
   * <span data-ttu-id="ef6c1-153">**Cloudové služby DNS název**: Toto je hello veřejného názvu DNS hello Cloudová služba, která souvisí s hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-153">**Cloud Service DNS Name**: This is hello public DNS name of hello Cloud Service that is associated with hello VM.</span></span> <span data-ttu-id="ef6c1-154">Hello výchozí název je hello název, který jste zadali v pro hello název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-154">hello default name is hello name you typed in for hello VM name.</span></span> <span data-ttu-id="ef6c1-155">Pokud v dalších krocích hello tématu, můžete vytvořit důvěryhodný certifikát SSL a pak se používá název DNS hello hello hodnotu hello "**vydané**" hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-155">If in later steps of hello topic, you create a trusted SSL certificate and then hello DNS name is used for hello value of hello “**Issued to**” of hello certificate.</span></span>
   * <span data-ttu-id="ef6c1-156">**Oblast nebo vztahů skupiny nebo virtuální síť**: Zvolte hello oblast nejbližší tooyour koncovým uživatelům.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-156">**Region/Affinity Group/Virtual Network**: Choose hello region closest tooyour end users.</span></span>
   * <span data-ttu-id="ef6c1-157">**Účet úložiště**: použít účet automaticky generované úložiště.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-157">**Storage Account**: Use an automatically generated storage account.</span></span>
   * <span data-ttu-id="ef6c1-158">**Skupina dostupnosti**: žádné.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-158">**Availability Set**: None.</span></span>
   * <span data-ttu-id="ef6c1-159">**Koncové body** zachovat hello **vzdálené plochy** a **prostředí PowerShell** koncových bodů a poté přidejte HTTP nebo HTTPS koncový bod, v závislosti na vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-159">**ENDPOINTS** Keep hello **Remote Desktop** and **PowerShell** endpoints and then add either an HTTP or HTTPS endpoint, depending on your environment.</span></span>
     
     * <span data-ttu-id="ef6c1-160">**HTTP**: hello výchozí veřejné a privátní porty jsou **80**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-160">**HTTP**: hello default public and private ports are **80**.</span></span> <span data-ttu-id="ef6c1-161">Všimněte si, že pokud používáte privátní port než 80, změňte **$HTTPport = 80** ve skriptu hello protokolu http.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-161">Note that if you use a private port other than 80, modify **$HTTPport = 80** in hello http script.</span></span>
     * <span data-ttu-id="ef6c1-162">**HTTPS**: hello výchozí veřejné a privátní porty jsou **443**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-162">**HTTPS**: hello default public and private ports are **443**.</span></span> <span data-ttu-id="ef6c1-163">Nejlepším postupem zabezpečení je toochange hello privátní port a nakonfigurujte vaší brány firewall a hello sestavy toouse hello privátní port serveru.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-163">A security best practice is toochange hello private port and configure your firewall and hello report server toouse hello private port.</span></span> <span data-ttu-id="ef6c1-164">Další informace o koncových bodů najdete v tématu [jak tooSet komunikace až s virtuálním počítačem](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-164">For more information on endpoints, see [How tooSet Up Communication with a Virtual Machine](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="ef6c1-165">Všimněte si, že pokud používáte jiný port než 443, změňte parametr hello **$HTTPsport = 443** v hello skriptu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-165">Note that if you use a port other than 443, change hello parameter **$HTTPsport = 443** in hello HTTPS script.</span></span>
   * <span data-ttu-id="ef6c1-166">Klikněte na tlačítko Další.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-166">Click next .</span></span> ![Další](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
8. <span data-ttu-id="ef6c1-168">Na poslední stránce hello hello Průvodce ponechat výchozí hello **instalace agenta virtuálního počítače hello** vybrané.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-168">On hello last page of hello wizard, keep hello default **Install hello VM agent** selected.</span></span> <span data-ttu-id="ef6c1-169">Hello kroků v tomto tématu není využívat hello agenta virtuálního počítače, ale pokud máte v plánu tookeep tento virtuální počítač, hello agenta virtuálního počítače a rozšíření vám umožní tooenhance he CM.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-169">hello steps in this topic do not utilize hello VM agent but if you plan tookeep this VM, hello VM agent and extensions will allow you tooenhance he CM.</span></span>  <span data-ttu-id="ef6c1-170">Další informace o hello agenta virtuálního počítače najdete v tématu [agenta virtuálního počítače a rozšíření – část 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-170">For more information on hello VM agent, see [VM Agent and Extensions – Part 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/).</span></span> <span data-ttu-id="ef6c1-171">Jeden z hello výchozí nainstalovaná rozšíření ad systémem je rozšíření "BGINFO" hello, která zobrazuje na ploše virtuálního počítače hello, jednotka systémové informace, jako je například interní IP a volné místo.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-171">One of hello default extensions installed ad running is hello “BGINFO” extension that displays on hello VM desktop, system information such as internal IP and free drive space.</span></span>
9. <span data-ttu-id="ef6c1-172">Klikněte na dokončení.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-172">Click complete .</span></span> ![Ok](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)
10. <span data-ttu-id="ef6c1-174">Hello **stav** z virtuálního počítače se zobrazí jako hello **počáteční (zřizování)** během procesu zřizování hello a potom zobrazí jako **systémem** Pokud hello virtuálního počítače je zřízený a připravené toouse.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-174">hello **Status** of hello VM displays as **Starting (Provisioning)** during hello provision process and then displays as **Running** when hello VM is provisioned and ready toouse.</span></span>

## <a name="step-2-create-a-server-certificate"></a><span data-ttu-id="ef6c1-175">Krok 2: Vytvoření certifikátu serveru</span><span class="sxs-lookup"><span data-stu-id="ef6c1-175">Step 2: Create a Server Certificate</span></span>
> [!NOTE]
> <span data-ttu-id="ef6c1-176">Pokud nechcete, aby HTTPS na serveru sestav hello, můžete **přeskočit krok 2** a přejděte toohello části **používat server sestav hello tooconfigure skriptu a HTTP**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-176">If you do not require HTTPS on hello report server, you can **skip step 2** and go toohello section **Use script tooconfigure hello report server and HTTP**.</span></span> <span data-ttu-id="ef6c1-177">Použití hello HTTP skriptu tooquickly nakonfigurovat server sestav hello a hello sestavy, že server bude připravená toouse.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-177">Use hello HTTP script tooquickly configure hello report server and hello report server will be ready toouse.</span></span>

<span data-ttu-id="ef6c1-178">Pořadí toouse HTTPS na hello virtuálních počítačů je nutné důvěryhodný certifikát SSL.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-178">In order toouse HTTPS on hello VM, you need a trusted SSL certificate.</span></span> <span data-ttu-id="ef6c1-179">V závislosti na scénáři můžete použít jednu z následujících dvou metod hello:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-179">Depending on your scenario, you can use one of hello following two methods:</span></span>

* <span data-ttu-id="ef6c1-180">Platný certifikát SSL vydaný certifikační autoritou (CA) a Microsoft za důvěryhodnou.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-180">A valid SSL certificate issued by a Certification Authority (CA) and trusted by Microsoft.</span></span> <span data-ttu-id="ef6c1-181">certifikáty kořenové certifikační Autority Hello jsou požadované toobe distribuovány prostřednictvím hello Microsoft Root Certificate Program.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-181">hello CA root certificates are required toobe distributed via hello Microsoft Root Certificate Program.</span></span> <span data-ttu-id="ef6c1-182">Další informace o tomto programu najdete v tématu [Windows a Windows Phone 8 SSL Root Certificate Program (člen certifikační autority)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) a [Úvod toohello Microsoft Root Certificate Program](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-182">For more information about this program, see [Windows and Windows Phone 8 SSL Root Certificate Program (Member CAs)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) and [Introduction toohello Microsoft Root Certificate Program](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).</span></span>
* <span data-ttu-id="ef6c1-183">Certifikát podepsaný svým držitelem.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-183">A self-signed certificate.</span></span> <span data-ttu-id="ef6c1-184">Certifikáty podepsané svým držitelem nejsou doporučuje pro provozní prostředí.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-184">Self-signed certificates are not recommended for production environments.</span></span>

### <a name="toouse-a-certificate-created-by-a-trusted-certificate-authority-ca"></a><span data-ttu-id="ef6c1-185">toouse vytvořit certifikát důvěryhodné certifikační autority (CA)</span><span class="sxs-lookup"><span data-stu-id="ef6c1-185">toouse a certificate created by a trusted Certificate Authority (CA)</span></span>
1. <span data-ttu-id="ef6c1-186">**Žádosti o certifikát serveru pro web hello od certifikační autority**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-186">**Request a server certificate for hello website from a certification authority**.</span></span> 
   
    <span data-ttu-id="ef6c1-187">Hello Průvodce certifikátem webového serveru můžete použít buď toogenerate soubor žádosti o certifikát (Certreq.txt), můžete odeslat tooa certifikační autoritou, nebo toogenerate požadavek pro online certifikační autoritu.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-187">You can use hello Web Server Certificate Wizard either toogenerate a certificate request file (Certreq.txt) that you send tooa certification authority, or toogenerate a request for an online certification authority.</span></span> <span data-ttu-id="ef6c1-188">Například certifikační služby společnosti Microsoft ve Windows serveru 2012.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-188">For example, Microsoft Certificate Services in Windows Server 2012.</span></span> <span data-ttu-id="ef6c1-189">V závislosti na úrovni hello identifikace záruky poskytované certifikátem serveru je několik měsíců dnů tooseveral hello certifikační autority tooapprove vaši žádost a odeslat je soubor certifikátu.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-189">Depending on hello level of identification assurance offered by your server certificate, it is several days tooseveral months for hello certification authority tooapprove your request and send you a certificate file.</span></span> 
   
    <span data-ttu-id="ef6c1-190">Další informace o vyžádání certifikátů serveru najdete v tématu hello následující:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-190">For more information about requesting a server certificates, see hello following:</span></span> 
   
   * <span data-ttu-id="ef6c1-191">Použití [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-191">Use [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).</span></span>
   * <span data-ttu-id="ef6c1-192">Nástroje pro tooAdminister zabezpečení systému Windows Server 2012.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-192">Security Tools tooAdminister Windows Server 2012.</span></span>
     
     [<span data-ttu-id="ef6c1-193">Nástroje pro tooAdminister zabezpečení systému Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="ef6c1-193">Security Tools tooAdminister Windows Server 2012</span></span>](https://technet.microsoft.com/library/jj730960.aspx)
     
     > [!NOTE]
     > <span data-ttu-id="ef6c1-194">Hello **vydané** pole hello důvěryhodný certifikát SSL by měl být hello stejné jako hello **název cloudové služby DNS** jste použili pro hello nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-194">hello **issued to** field of hello trusted SSL certificate should be hello same as hello **Cloud Service DNS NAME** you used for hello new VM.</span></span>

2. <span data-ttu-id="ef6c1-195">**Nainstalujte certifikát serveru hello na webový server hello**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-195">**Install hello server certificate on hello Web server**.</span></span> <span data-ttu-id="ef6c1-196">Hello webový server je v tomto případě hello virtuálních počítačů, hostitelů hello server sestav a hello web je vytvořen v dalších krocích při konfiguraci služby Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-196">hello Web server in this case is hello VM that hosts hello report server and hello website is created in later steps when you configure Reporting Services.</span></span> <span data-ttu-id="ef6c1-197">Další informace o instalaci certifikátu serveru hello hello webový server pomocí modulu snap-in konzoly MMC certifikát hello najdete v tématu [nainstalovat certifikát serveru](https://technet.microsoft.com/library/cc740068).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-197">For more information about installing hello server certificate on hello Web server by using hello Certificate MMC snap-in, see [Install a Server Certificate](https://technet.microsoft.com/library/cc740068).</span></span>
   
    <span data-ttu-id="ef6c1-198">Pokud chcete, aby toouse hello skript dodaný spolu s Toto téma, server sestav hello tooconfigure hello hodnota hello certifikátů **kryptografický otisk** se vyžaduje jako parametr hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-198">If you want toouse hello script included with this topic, tooconfigure hello report server, hello value of hello certificates **Thumbprint** is required as a parameter of hello script.</span></span> <span data-ttu-id="ef6c1-199">V části hello další podrobnosti o tom, jak tooobtain hello kryptografický otisk certifikátu hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-199">See hello next section for details on how tooobtain hello thumbprint of hello certificate.</span></span>
3. <span data-ttu-id="ef6c1-200">Přiřazení serveru sestav toohello certifikát server hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-200">Assign hello server certificate toohello report server.</span></span> <span data-ttu-id="ef6c1-201">přiřazení Hello byla dokončena v další části hello při konfiguraci serveru sestav hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-201">hello assignment is completed in hello next section when you configure hello report server.</span></span>

### <a name="toouse-hello-virtual-machines-self-signed-certificate"></a><span data-ttu-id="ef6c1-202">toouse hello certifikát podepsaný svým držitelem virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="ef6c1-202">toouse hello Virtual Machines Self-signed Certificate</span></span>
<span data-ttu-id="ef6c1-203">Certifikát podepsaný svým držitelem byl vytvořen na hello virtuálních počítačů při zřizování hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-203">A self-signed certificate was created on hello VM when hello VM was provisioned.</span></span> <span data-ttu-id="ef6c1-204">Hello certifikát má stejný název jako název DNS virtuálního počítače hello hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-204">hello certificate has hello same name as hello VM DNS name.</span></span> <span data-ttu-id="ef6c1-205">V pořadí tooavoid certifikát chyby, není zapotřebí hello certifikátu je důvěryhodný na hello virtuální počítač a také všichni uživatelé hello lokality.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-205">In order tooavoid certificate errors, it is required that hello certificate is trusted on hello VM itself, and also by all users of hello site.</span></span>

1. <span data-ttu-id="ef6c1-206">tootrust hello kořenové certifikační Autority certifikátu hello na hello Virtuálního počítače, přidejte hello certifikát toohello **důvěryhodné kořenové certifikační autority**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-206">tootrust hello root CA of hello certificate on hello Local VM, add hello certificate toohello **Trusted Root Certification Authorities**.</span></span> <span data-ttu-id="ef6c1-207">Hello Následuje souhrn hello kroky.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-207">hello following is a summary of hello steps required.</span></span> <span data-ttu-id="ef6c1-208">Podrobné kroky na tom, jak tootrust hello certifikační Autority najdete v tématu [nainstalovat certifikát serveru](https://technet.microsoft.com/library/cc740068).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-208">For detailed steps on how tootrust hello CA, see [Install a Server Certificate](https://technet.microsoft.com/library/cc740068).</span></span>
   
   1. <span data-ttu-id="ef6c1-209">Hello portál Azure classic, vyberte hello virtuálních počítačů a klikněte na tlačítko Připojit.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-209">From hello Azure classic portal, select hello VM and click connect.</span></span> <span data-ttu-id="ef6c1-210">V závislosti na konfiguraci vašeho prohlížeče může být výzvami toosave soubor RDP pro připojení toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-210">Depending on your browser configuration, you may be prompted toosave an .rdp file for connecting toohello VM.</span></span>
      
       ![připojit tooazure virtuálního počítače](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) <span data-ttu-id="ef6c1-212">Použijte název virtuálního počítače hello uživatele, uživatelské jméno a heslo, které jste nakonfigurovali při vytváření hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-212">Use hello user VM name, user name and password you configured when you created hello VM.</span></span> 
      
       <span data-ttu-id="ef6c1-213">Například v hello následující bitové kopie, je název virtuálního počítače hello **ssrsnativecloud** a hello uživatelské jméno je **testuser**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-213">For example, in hello following image, hello VM name is **ssrsnativecloud** and hello user name is **testuser**.</span></span>
      
       ![název virtuálního počítače jejíž součástí přihlášení](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
   2. <span data-ttu-id="ef6c1-215">Spusťte mmc.exe.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-215">Run mmc.exe.</span></span> <span data-ttu-id="ef6c1-216">Další informace najdete v tématu [postupy: zobrazení certifikátů pomocí modulu Snap-in konzoly MMC hello](https://msdn.microsoft.com/library/ms788967.aspx).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-216">For more information, see [How to: View Certificates with hello MMC Snap-in](https://msdn.microsoft.com/library/ms788967.aspx).</span></span>
   3. <span data-ttu-id="ef6c1-217">V konzole aplikace hello **soubor** nabídce Přidat hello **certifikáty** modul snap-in, vyberte **účet počítače** při zobrazení výzvy a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-217">In hello console application **File** menu, add hello **Certificates** snap-in, select **Computer Account** when prompted, and then click **Next**.</span></span>
   4. <span data-ttu-id="ef6c1-218">Vyberte **místního počítače** toomanage a pak klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-218">Select **Local Computer** toomanage and then click **Finish**.</span></span>
   5. <span data-ttu-id="ef6c1-219">Klikněte na tlačítko **Ok** a potom rozbalte hello **certifikáty – osobní** uzly a pak klikněte na tlačítko **certifikáty**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-219">Click **Ok** and then expand hello **Certificates -Personal** nodes and then click **Certificates**.</span></span> <span data-ttu-id="ef6c1-220">Hello certifikát jmenuje po název DNS hello hello virtuálních počítačů a končí **cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-220">hello certificate is named after hello DNS name of hello VM and ends with **cloudapp.net**.</span></span> <span data-ttu-id="ef6c1-221">Klikněte pravým tlačítkem na název hello certifikátu a klikněte na tlačítko **kopie**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-221">Right-click hello certificate name and click **Copy**.</span></span>
   6. <span data-ttu-id="ef6c1-222">Rozbalte hello **důvěryhodné kořenové certifikační autority** uzel a potom klikněte pravým tlačítkem na **certifikáty** a pak klikněte na **vložení**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-222">Expand hello **Trusted Root Certification Authorities** node and then right-click **Certificates** and then click **Paste**.</span></span>
   7. <span data-ttu-id="ef6c1-223">toovalidate, double klikněte na název certifikátu hello pod **důvěryhodné kořenové certifikační autority** a ověřte, že nejsou žádné chyby a svůj certifikát.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-223">toovalidate, double click on hello certificate name under **Trusted Root Certification Authorities** and verify that there are no errors and you see your certificate.</span></span> <span data-ttu-id="ef6c1-224">Pokud chcete, aby toouse hello HTTPS skript dodaný spolu s Toto téma, server sestav hello tooconfigure hello hodnota hello certifikátů **kryptografický otisk** se vyžaduje jako parametr hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-224">If you want toouse hello HTTPS script included with this topic, tooconfigure hello report server, hello value of hello certificates **Thumbprint** is required as a parameter of hello script.</span></span> <span data-ttu-id="ef6c1-225">**Hodnota kryptografického otisku hello tooget**, dokončete následující hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-225">**tooget hello thumbprint value**, complete hello following.</span></span> <span data-ttu-id="ef6c1-226">V části je také prostředí PowerShell ukázkový tooretrieve hello kryptografický otisk [používat server sestav hello tooconfigure skriptu a HTTPS](#use-script-to-configure-the-report-server-and-HTTPS).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-226">There is also a PowerShell sample tooretrieve hello thumbprint in section [Use script tooconfigure hello report server and HTTPS](#use-script-to-configure-the-report-server-and-HTTPS).</span></span>
      
      1. <span data-ttu-id="ef6c1-227">Klikněte dvakrát na název hello hello certifikátu, například ssrsnativecloud.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-227">Double-click hello name of hello certificate, for example ssrsnativecloud.cloudapp.net.</span></span>
      2. <span data-ttu-id="ef6c1-228">Klikněte na tlačítko hello **podrobnosti** kartě.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-228">Click hello **Details** tab.</span></span>
      3. <span data-ttu-id="ef6c1-229">Klikněte na tlačítko **kryptografický otisk**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-229">Click **Thumbprint**.</span></span> <span data-ttu-id="ef6c1-230">v poli Podrobnosti hello, například a6 se zobrazí hodnota Hello hello kryptografický otisk 08 3c df f9 0b f7 e3 7c 25 ed a4 ed 7e ac 91 9c 2c fb 2f.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-230">hello value of hello thumbprint is displayed in hello details field, for example ‎a6 08 3c df f9 0b f7 e3 7c 25 ed a4 ed 7e ac 91 9c 2c fb 2f.</span></span>
      4. <span data-ttu-id="ef6c1-231">Zkopírujte kryptografický otisk hello a uložit pro pozdější hello hodnotu nebo upravte skript hello teď.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-231">Copy hello thumbprint and save hello value for later or edit hello script now.</span></span>
      5. <span data-ttu-id="ef6c1-232">(*) Před spuštěním skriptu hello odeberte hello mezery mezi hello dvojice hodnot.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-232">(*) Before you run hello script, remove hello spaces in between hello pairs of values.</span></span> <span data-ttu-id="ef6c1-233">Například kryptografický otisk hello zjištěno před by nyní a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-233">For example hello thumbprint noted before would now be ‎a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f.</span></span>
      6. <span data-ttu-id="ef6c1-234">Přiřazení serveru sestav toohello certifikát server hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-234">Assign hello server certificate toohello report server.</span></span> <span data-ttu-id="ef6c1-235">přiřazení Hello byla dokončena v další části hello při konfiguraci serveru sestav hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-235">hello assignment is completed in hello next section when you configure hello report server.</span></span>

<span data-ttu-id="ef6c1-236">Pokud používáte certifikát podepsaný svým držitelem SSL, název hello na hello certifikátu již odpovídá hello název hostitele hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-236">If you are using a self-signed SSL certificate, hello name on hello certificate already matches hello hostname of hello VM.</span></span> <span data-ttu-id="ef6c1-237">Proto hello DNS počítače hello je již zaregistrován globálně a je přístupná z libovolného klienta.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-237">Therefore, hello DNS of hello machine is already registered globally and can be accessed from any client.</span></span>

## <a name="step-3-configure-hello-report-server"></a><span data-ttu-id="ef6c1-238">Krok 3: Konfigurace hello serveru sestav</span><span class="sxs-lookup"><span data-stu-id="ef6c1-238">Step 3: Configure hello Report Server</span></span>
<span data-ttu-id="ef6c1-239">Tato část vás provede procesem konfigurace hello virtuální počítač jako server sestav služby Reporting Services v nativním režimu.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-239">This section walks you through configuring hello VM as a Reporting Services native mode report server.</span></span> <span data-ttu-id="ef6c1-240">Můžete použít jednu z hello následující server sestav hello tooconfigure metody:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-240">You can use one of hello following methods tooconfigure hello report server:</span></span>

* <span data-ttu-id="ef6c1-241">Používat server sestav hello skriptu tooconfigure hello</span><span class="sxs-lookup"><span data-stu-id="ef6c1-241">Use hello script tooconfigure hello report server</span></span>
* <span data-ttu-id="ef6c1-242">Použijte nástroj Configuration Manager tooConfigure hello serveru sestav.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-242">Use Configuration Manager tooConfigure hello Report Server.</span></span>

<span data-ttu-id="ef6c1-243">Podrobné kroky, najdete v části hello [toohello připojit virtuální počítač a spusťte Správce konfigurace služby Reporting Services hello](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-243">For more detailed steps, see hello section [Connect toohello Virtual Machine and Start hello Reporting Services Configuration Manager](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).</span></span>

<span data-ttu-id="ef6c1-244">**Poznámka: ověřování:** ověřování systému Windows je doporučená metoda ověřování hello a je hello výchozí služby Reporting Services ověřování.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-244">**Authentication Note:** Windows authentication is hello recommended authentication method and it is hello default Reporting Services authentication.</span></span> <span data-ttu-id="ef6c1-245">Pouze uživatelé, kteří jsou nakonfigurované na hello virtuálních počítačů přístup k službě Reporting Services a přiřazené tooReporting služeb role.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-245">Only users that are configured on hello VM can access Reporting Services and assigned tooReporting Services roles.</span></span>

### <a name="use-script-tooconfigure-hello-report-server-and-http"></a><span data-ttu-id="ef6c1-246">Server sestav pomocí skriptu tooconfigure hello a HTTP</span><span class="sxs-lookup"><span data-stu-id="ef6c1-246">Use script tooconfigure hello report server and HTTP</span></span>
<span data-ttu-id="ef6c1-247">toouse hello prostředí Windows PowerShell skriptu tooconfigure hello serveru sestav, dokončení hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-247">toouse hello Windows PowerShell script tooconfigure hello report server, complete hello following steps.</span></span> <span data-ttu-id="ef6c1-248">Konfigurace Hello obsahuje protokolu HTTP, nikoli HTTPS:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-248">hello configuration includes HTTP, not HTTPS:</span></span>

1. <span data-ttu-id="ef6c1-249">Hello portál Azure classic, vyberte hello virtuálních počítačů a klikněte na tlačítko Připojit.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-249">From hello Azure classic portal, select hello VM and click connect.</span></span> <span data-ttu-id="ef6c1-250">V závislosti na konfiguraci vašeho prohlížeče může být výzvami toosave soubor RDP pro připojení toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-250">Depending on your browser configuration, you may be prompted toosave an .rdp file for connecting toohello VM.</span></span>
   
    ![připojit tooazure virtuálního počítače](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) <span data-ttu-id="ef6c1-252">Použijte název virtuálního počítače hello uživatele, uživatelské jméno a heslo, které jste nakonfigurovali při vytváření hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-252">Use hello user VM name, user name and password you configured when you created hello VM.</span></span> 
   
    <span data-ttu-id="ef6c1-253">Například v hello následující bitové kopie, je název virtuálního počítače hello **ssrsnativecloud** a hello uživatelské jméno je **testuser**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-253">For example, in hello following image, hello VM name is **ssrsnativecloud** and hello user name is **testuser**.</span></span>
   
    ![název virtuálního počítače jejíž součástí přihlášení](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. <span data-ttu-id="ef6c1-255">Na hello virtuálních počítačů, otevřete **Windows PowerShell ISE** s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-255">On hello VM, open **Windows PowerShell ISE** with administrative privileges.</span></span> <span data-ttu-id="ef6c1-256">Hello PowerShell ISE je nainstalována ve výchozím nastavení v systému Windows server 2012.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-256">hello PowerShell ISE is installed by default on Windows server 2012.</span></span> <span data-ttu-id="ef6c1-257">Doporučuje se, že používáte hello ISE místo standardní okno prostředí Windows PowerShell, aby mohli vložit hello skriptu do hello ISE, upravte hello skript a spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-257">It is recommended you use hello ISE instead of a standard Windows PowerShell window so that you can paste hello script into hello ISE, modify hello script, and then run hello script.</span></span>
3. <span data-ttu-id="ef6c1-258">V systému Windows PowerShell ISE, klikněte na tlačítko hello **zobrazení** nabídce a pak klikněte na tlačítko **zobrazit podokno skriptu**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-258">In Windows PowerShell ISE, click hello **View** menu and then click **Show Script Pane**.</span></span>
4. <span data-ttu-id="ef6c1-259">Zkopírujte hello následující skript a vložte hello skriptu do podokno skriptu Windows PowerShell ISE hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-259">Copy hello following script, and paste hello script into hello Windows PowerShell ISE script pane.</span></span>
   
        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
   
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change hello value if you used a different port for hello private HTTP endpoint when hello VM was created.
   
        ## Set PowerShell execution policy toobe able toorun scripts
        Set-ExecutionPolicy RemoteSigned -Force
   
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
   
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
   
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
   
        ## Register for MSReportServer_ConfigurationSetting
        ## Change hello version portion of hello path too"v11" toouse hello script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Report Server Configuration Steps
   
        ## Setting hello web service URL ##
        write-host -foregroundcolor green "Setting hello web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
   
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
   
        ## Setting hello Database ##
        write-host -foregroundcolor green "Setting hello Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating hello database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script toocreate hello database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes toosqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
   
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
   
        ## Setting hello Report Manager URL ##
   
        write-host -foregroundcolor green "Setting hello Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
   
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
   
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
   
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
5. <span data-ttu-id="ef6c1-260">Pokud jste vytvořili hello virtuálních počítačů s k portu HTTP než 80, upravte parametr hello $HTTPport = 80.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-260">If you created hello VM with an HTTP port other than 80, modify hello parameter $HTTPport = 80.</span></span>
6. <span data-ttu-id="ef6c1-261">skript Hello je aktuálně konfigurována pro službu Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-261">hello script is currently configured for  Reporting Services.</span></span> <span data-ttu-id="ef6c1-262">Pokud chcete toorun hello skript pro službu Reporting Services, upravte část verze hello hello cesta toohello oboru názvů příliš verze "11", na příkaz Get-WmiObject hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-262">If you want toorun hello script for  Reporting Services, modify hello version portion of hello path toohello namespace too“v11”, on hello Get-WmiObject statement.</span></span>
7. <span data-ttu-id="ef6c1-263">Spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-263">Run hello script.</span></span>

<span data-ttu-id="ef6c1-264">**Ověření**: tooverify, který funkce serveru základní sestavy hello funguje, najdete v části hello [konfigurace ověřte, zda text hello](#verify-the-configuration) později v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-264">**Validation**: tooverify that hello basic report server functionality is working, see hello [Verify hello configuration](#verify-the-configuration) section later in this topic.</span></span>

### <a name="use-script-tooconfigure-hello-report-server-and-https"></a><span data-ttu-id="ef6c1-265">Server sestav pomocí skriptu tooconfigure hello a HTTPS</span><span class="sxs-lookup"><span data-stu-id="ef6c1-265">Use script tooconfigure hello report server and HTTPS</span></span>
<span data-ttu-id="ef6c1-266">toouse prostředí Windows PowerShell tooconfigure hello serveru sestav, dokončení hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-266">toouse Windows PowerShell tooconfigure hello report server, complete hello following steps.</span></span> <span data-ttu-id="ef6c1-267">zahrnuje Hello konfigurace protokolu HTTPS, nikoli protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-267">hello configuration includes HTTPS, not HTTP.</span></span>

1. <span data-ttu-id="ef6c1-268">Hello portál Azure classic, vyberte hello virtuálních počítačů a klikněte na tlačítko Připojit.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-268">From hello Azure classic portal, select hello VM and click connect.</span></span> <span data-ttu-id="ef6c1-269">V závislosti na konfiguraci vašeho prohlížeče může být výzvami toosave soubor RDP pro připojení toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-269">Depending on your browser configuration, you may be prompted toosave an .rdp file for connecting toohello VM.</span></span>
   
    ![připojit tooazure virtuálního počítače](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) <span data-ttu-id="ef6c1-271">Použijte název virtuálního počítače hello uživatele, uživatelské jméno a heslo, které jste nakonfigurovali při vytváření hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-271">Use hello user VM name, user name and password you configured when you created hello VM.</span></span> 
   
    <span data-ttu-id="ef6c1-272">Například v hello následující bitové kopie, je název virtuálního počítače hello **ssrsnativecloud** a hello uživatelské jméno je **testuser**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-272">For example, in hello following image, hello VM name is **ssrsnativecloud** and hello user name is **testuser**.</span></span>
   
    ![název virtuálního počítače jejíž součástí přihlášení](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. <span data-ttu-id="ef6c1-274">Na hello virtuálních počítačů, otevřete **Windows PowerShell ISE** s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-274">On hello VM, open **Windows PowerShell ISE** with administrative privileges.</span></span> <span data-ttu-id="ef6c1-275">Hello PowerShell ISE je nainstalována ve výchozím nastavení v systému Windows server 2012.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-275">hello PowerShell ISE is installed by default on Windows server 2012.</span></span> <span data-ttu-id="ef6c1-276">Doporučuje se, že používáte hello ISE místo standardní okno prostředí Windows PowerShell, aby mohli vložit hello skriptu do hello ISE, upravte hello skript a spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-276">It is recommended you use hello ISE instead of a standard Windows PowerShell window so that you can paste hello script into hello ISE, modify hello script, and then run hello script.</span></span>
3. <span data-ttu-id="ef6c1-277">tooenable spouštění skriptů, spusťte následující příkaz prostředí Windows PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-277">tooenable running scripts, run hello following Windows PowerShell command:</span></span>
   
        Set-ExecutionPolicy RemoteSigned
   
    <span data-ttu-id="ef6c1-278">Potom můžete spustit hello následující tooverify hello zásad:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-278">You can then run hello following tooverify hello policy:</span></span>
   
        Get-ExecutionPolicy
4. <span data-ttu-id="ef6c1-279">V **Windows PowerShell ISE**, klikněte na tlačítko hello **zobrazení** nabídce a pak klikněte na tlačítko **zobrazit podokno skriptu**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-279">In **Windows PowerShell ISE**, click hello **View** menu and then click **Show Script Pane**.</span></span>
5. <span data-ttu-id="ef6c1-280">Zkopírujte následující skript a vložte jej do podokno skriptu Windows PowerShell ISE hello hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-280">Copy hello following script and paste it into hello Windows PowerShell ISE script pane.</span></span>
   
        ## This script configures hello report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when hello HTTPS endpoint was created.
   
        # You can run hello following command tooget (.cloudapp.net certificates) so you can copy hello thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # hello certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # hello certificate hash should not contain spaces
   
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If hello certificate is not a wildcard certificate, comment out hello following line, and enable hello full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
   
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
   
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
   
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
   
        write-host "hello script will use $DNSNameAndPort as hello DNS name and port" 
   
        ## Register for MSReportServer_ConfigurationSetting
        ## Change hello version portion of hello path too"v11" toouse hello script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Reporting Services Report Server Configuration Steps
   
        ## 1. Setting hello web service URL ##
        write-host -foregroundcolor green "Setting hello web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
   
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
   
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
   
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
   
        ## 2. Setting hello Database ##
        write-host -foregroundcolor green "Setting hello Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating hello database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script toocreate hello database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes toosqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
   
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
   
        ## 3. Setting hello Report Manager URL ##
   
        write-host -foregroundcolor green "Setting hello Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
   
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
   
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
   
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
   
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
   
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
6. <span data-ttu-id="ef6c1-281">Upravit hello **$certificatehash** parametr ve skriptu hello:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-281">Modify hello **$certificatehash** parameter in hello script:</span></span>
   
   * <span data-ttu-id="ef6c1-282">Toto je **požadované** parametr.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-282">This is a **required** parameter.</span></span> <span data-ttu-id="ef6c1-283">Pokud hodnotu hello certifikátu nebyla uložena z předchozích kroků hello, použijte jednu z následujících metod toocopy hello hodnotu hash pro certifikát z kryptografický otisk certifikáty hello hello.:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-283">If you did not save hello certificate value from hello previous steps, use one of hello following methods toocopy hello certificate hash value from hello certificates thumbprint.:</span></span>
     
       <span data-ttu-id="ef6c1-284">Na hello virtuálních počítačů otevřete Windows PowerShell ISE a spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-284">On hello VM, open Windows PowerShell ISE and run hello following command:</span></span>
     
           dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
     
       <span data-ttu-id="ef6c1-285">Hello výstup bude vypadat podobně jako následující toohello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-285">hello output will look similar toohello following.</span></span> <span data-ttu-id="ef6c1-286">Pokud hello skript vrátí prázdný řádek, hello virtuální počítač nemá certifikát třeba nakonfigurovat, najdete v tématu hello [toouse hello certifikát podepsaný svým držitelem virtuálních počítačů](#to-use-the-virtual-machines-self-signed-certificate).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-286">If hello script returns a blank line, hello VM does not have a certificate configured for example, see hello section [toouse hello Virtual Machines Self-signed Certificate](#to-use-the-virtual-machines-self-signed-certificate).</span></span>
     
     <span data-ttu-id="ef6c1-287">NEBO</span><span class="sxs-lookup"><span data-stu-id="ef6c1-287">OR</span></span>
   * <span data-ttu-id="ef6c1-288">Hello mmc.exe spustit virtuální počítač a poté přidejte hello **certifikáty** modul snap-in.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-288">On hello VM Run mmc.exe and then add hello **Certificates** snap-in.</span></span>
   * <span data-ttu-id="ef6c1-289">V části hello **důvěryhodné kořenové certifikační úřady** uzlu, dvakrát klikněte na název certifikátu.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-289">Under hello **Trusted Root Certificate Authorities** node double-click your certificate name.</span></span> <span data-ttu-id="ef6c1-290">Pokud používáte certifikát podepsaný svým držitelem hello hello virtuálních počítačů, certifikát hello jmenuje po název DNS hello hello virtuálních počítačů a končí **cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-290">If you are using hello self-signed certificate of hello VM, hello certificate is named after hello DNS name of hello VM and ends with **cloudapp.net**.</span></span>
   * <span data-ttu-id="ef6c1-291">Klikněte na tlačítko hello **podrobnosti** kartě.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-291">Click hello **Details** tab.</span></span>
   * <span data-ttu-id="ef6c1-292">Klikněte na tlačítko **kryptografický otisk**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-292">Click **Thumbprint**.</span></span> <span data-ttu-id="ef6c1-293">Hodnota Hello kryptografický otisk hello je zobrazena v hello pole podrobností, například af 11 60 b6 4b 28 8 d 89 0a 82 12 ff 6b a9 c3 66 4f 31 90 48</span><span class="sxs-lookup"><span data-stu-id="ef6c1-293">hello value of hello thumbprint is displayed in hello details field, for example af 11 60 b6 4b 28 8d 89 0a 82 12 ff 6b a9 c3 66 4f 31 90 48</span></span>
   * <span data-ttu-id="ef6c1-294">**Před spuštěním skriptu hello**, odebrat hello mezery mezi hello dvojice hodnot.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-294">**Before you run hello script**, remove hello spaces in between hello pairs of values.</span></span> <span data-ttu-id="ef6c1-295">Například af1160b64b288d890a8212ff6ba9c3664f319048</span><span class="sxs-lookup"><span data-stu-id="ef6c1-295">For example af1160b64b288d890a8212ff6ba9c3664f319048</span></span>
7. <span data-ttu-id="ef6c1-296">Upravit hello **$httpsport** parametr:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-296">Modify hello **$httpsport** parameter:</span></span> 
   
   * <span data-ttu-id="ef6c1-297">Pokud jste použili port 443 pro koncový bod HTTPS hello, pak nepotřebujete tooupdate tento parametr ve skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-297">If you used port 443 for hello HTTPS endpoint, then you do not need tooupdate this parameter in hello script.</span></span> <span data-ttu-id="ef6c1-298">Jinak použijte hello port hodnotu, kterou jste vybrali při konfiguraci privátní koncový bod HTTPS hello na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-298">Otherwise use hello port value you selected when you configured hello HTTPS private endpoint on hello VM.</span></span>
8. <span data-ttu-id="ef6c1-299">Upravit hello **$DNSName** parametr:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-299">Modify hello **$DNSName** parameter:</span></span> 
   
   * <span data-ttu-id="ef6c1-300">skript Hello je nakonfigurován pro zástupný certifikát $DNSName = "+".</span><span class="sxs-lookup"><span data-stu-id="ef6c1-300">hello script is configured for a wild card certificate $DNSName="+".</span></span> <span data-ttu-id="ef6c1-301">Pokud provádíte žádné tooconfigure chcete pro vazbu certifikátu zástupný znak, komentář $DNSName = "+"a povolte následující řádek, hello úplné $DNSNAme odkaz, ## $DNSName="$server.cloudapp.net hello".</span><span class="sxs-lookup"><span data-stu-id="ef6c1-301">If you do no want tooconfigure for a wildcard certificate binding, comment out $DNSName="+" and enable hello following line, hello full $DNSNAme reference, ##$DNSName="$server.cloudapp.net".</span></span>
     
       <span data-ttu-id="ef6c1-302">Změňte hodnotu hello $DNSName, pokud nechcete, aby název DNS toouse hello virtuálního počítače pro službu Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-302">Change hello $DNSName value if you do not want toouse hello virtual machine’s DNS name for Reporting Services.</span></span> <span data-ttu-id="ef6c1-303">Pokud použijete parametr hello, hello certifikát musíte taky použít tento název a zaregistrujte název hello globálně na serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-303">If you use hello parameter, hello certificate must also use this name and you register hello name globally on a DNS server.</span></span>
9. <span data-ttu-id="ef6c1-304">skript Hello je aktuálně konfigurována pro službu Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-304">hello script is currently configured for  Reporting Services.</span></span> <span data-ttu-id="ef6c1-305">Pokud chcete toorun hello skript pro službu Reporting Services, upravte část verze hello hello cesta toohello oboru názvů příliš verze "11", na příkaz Get-WmiObject hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-305">If you want toorun hello script for  Reporting Services, modify hello version portion of hello path toohello namespace too“v11”, on hello Get-WmiObject statement.</span></span>
10. <span data-ttu-id="ef6c1-306">Spusťte skript hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-306">Run hello script.</span></span>

<span data-ttu-id="ef6c1-307">**Ověření**: tooverify, který funkce serveru základní sestavy hello funguje, najdete v části hello [konfigurace ověřte, zda text hello](#verify-the-connection) později v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-307">**Validation**: tooverify that hello basic report server functionality is working, see hello [Verify hello configuration](#verify-the-connection) section later in this topic.</span></span> <span data-ttu-id="ef6c1-308">vazbu certifikátu hello tooverify otevřete příkazový řádek s oprávněními správce a spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-308">tooverify hello certificate binding open a command prompt with administrative privileges, and then run hello following command:</span></span>

    netsh http show sslcert

<span data-ttu-id="ef6c1-309">Hello výsledek bude obsahovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-309">hello result will include hello following:</span></span>

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-tooconfigure-hello-report-server"></a><span data-ttu-id="ef6c1-310">Použijte nástroj Configuration Manager tooConfigure hello serveru sestav</span><span class="sxs-lookup"><span data-stu-id="ef6c1-310">Use Configuration Manager tooConfigure hello Report Server</span></span>
<span data-ttu-id="ef6c1-311">Pokud nechcete, aby toorun hello prostředí PowerShell skriptu tooconfigure hello serveru sestav, postupujte podle kroků hello v této části toouse hello služby Reporting Services v nativním režimu tooconfigure hello sestavy serveru nástroje configuration manager.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-311">If you do not want toorun hello PowerShell script tooconfigure hello report server, follow hello steps in this section toouse hello Reporting Services native mode configuration manager tooconfigure hello report server.</span></span>

1. <span data-ttu-id="ef6c1-312">Hello portál Azure classic, vyberte hello virtuálních počítačů a klikněte na tlačítko Připojit.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-312">From hello Azure classic portal, select hello VM and click connect.</span></span> <span data-ttu-id="ef6c1-313">Použití hello uživatelské jméno a heslo, které jste nakonfigurovali při vytváření hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-313">Use hello user name and password you configured when you created hello VM.</span></span>
   
    ![připojit tooazure virtuálního počítače](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)
2. <span data-ttu-id="ef6c1-315">Spusťte službu Windows update a nainstalovat aktualizace toohello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-315">Run Windows update and install updates toohello VM.</span></span> <span data-ttu-id="ef6c1-316">Pokud se vyžaduje restartování systému hello virtuálních počítačů, restartujte hello virtuálních počítačů a znovu připojit toohello virtuálních počítačů z hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-316">If a restart of hello VM is required, restart hello VM and reconnect toohello VM from hello Azure classic portal.</span></span>
3. <span data-ttu-id="ef6c1-317">Z nabídky Start hello na hello virtuálního počítače, zadejte **služby Reporting Services** a otevřete **Správce konfigurace služby Reporting Services**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-317">From hello Start menu on hello VM, type **Reporting Services** and open **Reporting Services Configuration Manager**.</span></span>
4. <span data-ttu-id="ef6c1-318">Nechte hello výchozí hodnoty pro **název serveru** a **instanci serveru sestav**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-318">Leave hello default values for **Server Name** and **Report Server Instance**.</span></span> <span data-ttu-id="ef6c1-319">Klikněte na **Připojit**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-319">Click **Connect**.</span></span>
5. <span data-ttu-id="ef6c1-320">V levém podokně hello, klikněte na **adresa URL webové služby**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-320">In hello left pane, click **Web Service URL**.</span></span>
6. <span data-ttu-id="ef6c1-321">Ve výchozím nastavení je RS nakonfigurován pro protokol HTTP port 80 s IP Adresou "Všechny přiřazené".</span><span class="sxs-lookup"><span data-stu-id="ef6c1-321">By default, RS is configured for HTTP port 80 with IP “All Assigned”.</span></span> <span data-ttu-id="ef6c1-322">tooadd HTTPS:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-322">tooadd HTTPS:</span></span>
   
   1. <span data-ttu-id="ef6c1-323">V **certifikát SSL**: Vyberte hello certifikát má toouse, například [název virtuálního počítače]. cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-323">In **SSL Certificate**: select hello certificate you want toouse, for example [VM name].cloudapp.net.</span></span> <span data-ttu-id="ef6c1-324">Pokud nejsou uvedeny žádné certifikáty, najdete v tématu hello **krok 2: vytvoření certifikátu serveru** informace o tom, jak tooinstall a vztah důvěryhodnosti hello certifikát na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-324">If no certificates are listed, see hello section **Step 2: Create a Server Certificate** for information on how tooinstall and trust hello certificate on hello VM.</span></span>
   2. <span data-ttu-id="ef6c1-325">V části **SSL Port**: Zvolte 443.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-325">Under **SSL Port**: choose 443.</span></span> <span data-ttu-id="ef6c1-326">Pokud jste nakonfigurovali privátní koncový bod HTTPS hello v hello virtuálního počítače s jinou privátní port, použijte tuto hodnotu sem.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-326">If you configured hello HTTPS private endpoint in hello VM with a different private port, use that value here.</span></span>
   3. <span data-ttu-id="ef6c1-327">Klikněte na tlačítko **použít** a počkejte toocomplete operaci hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-327">Click **Apply** and wait for hello operation toocomplete.</span></span>
7. <span data-ttu-id="ef6c1-328">V levém podokně hello, klikněte na **databáze**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-328">In hello left pane, click **Database**.</span></span>
   
   1. <span data-ttu-id="ef6c1-329">Klikněte na tlačítko **změnit Databas**e.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-329">Click **Change Databas**e.</span></span>
   2. <span data-ttu-id="ef6c1-330">Klikněte na tlačítko **vytvořit novou databázi serveru sestav** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-330">Click **Create a new report server database** and then click **Next**.</span></span>
   3. <span data-ttu-id="ef6c1-331">Ponechte výchozí hello **název serveru**: jako hello virtuálního počítače zadejte název a ponechte výchozí hello **typ ověřování** jako **aktuální uživatel** – **integrované zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-331">Leave hello default **Server Name**: as hello VM name and leave hello default **Authentication Type** as **Current User** – **Integrated Security**.</span></span> <span data-ttu-id="ef6c1-332">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-332">Click **Next**.</span></span>
   4. <span data-ttu-id="ef6c1-333">Ponechte výchozí hello **název databáze** jako **ReportServer** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-333">Leave hello default **Database Name** as **ReportServer** and click **Next**.</span></span>
   5. <span data-ttu-id="ef6c1-334">Ponechte výchozí hello **typ ověřování** jako **pověření služby** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-334">Leave hello default **Authentication Type** as **Service Credentials** and click **Next**.</span></span>
   6. <span data-ttu-id="ef6c1-335">Klikněte na tlačítko **Další** na hello **Souhrn** stránky.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-335">Click **Next** on hello **Summary** page.</span></span>
   7. <span data-ttu-id="ef6c1-336">Po dokončení konfigurace hello klikněte na tlačítko **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-336">When hello configuration is complete, click **Finish**.</span></span>
8. <span data-ttu-id="ef6c1-337">V levém podokně hello, klikněte na **adresa URL správce sestav**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-337">In hello left pane, click **Report Manager URL**.</span></span> <span data-ttu-id="ef6c1-338">Ponechte výchozí hello **virtuální adresář** jako **sestavy** a klikněte na tlačítko **použít**.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-338">Leave hello default **Virtual Directory** as **Reports** and click **Apply**.</span></span>
9. <span data-ttu-id="ef6c1-339">Klikněte na tlačítko **ukončení** tooclose hello Správce konfigurace služby Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-339">Click **Exit** tooclose hello Reporting Services Configuration Manager.</span></span>

## <a name="step-4-open-windows-firewall-port"></a><span data-ttu-id="ef6c1-340">Krok 4: Port brány Firewall systému Windows otevřete</span><span class="sxs-lookup"><span data-stu-id="ef6c1-340">Step 4: Open Windows Firewall Port</span></span>
> [!NOTE]
> <span data-ttu-id="ef6c1-341">Pokud jste použili jedno hello skripty tooconfigure hello sestavy serveru, můžete tuto část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-341">If you used one of hello scripts tooconfigure hello report server, you can skip this section.</span></span> <span data-ttu-id="ef6c1-342">skript Hello zahrnuty port brány firewall krok tooopen hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-342">hello script included a step tooopen hello firewall port.</span></span> <span data-ttu-id="ef6c1-343">Výchozí Hello se port 80 pro protokol HTTP a port 443 pro protokol HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-343">hello default was port 80 for HTTP and port 443 for HTTPS.</span></span>
> 
> 

<span data-ttu-id="ef6c1-344">tooconnect vzdáleně tooReport Manager nebo hello zprávu Server hello virtuálního počítače, koncový bod TCP je vyžadován na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-344">tooconnect remotely tooReport Manager or hello Report Server on hello virtual machine, a TCP Endpoint is required on hello VM.</span></span> <span data-ttu-id="ef6c1-345">Je požadovaná tooopen hello stejný port v bráně firewall hello Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-345">It is required tooopen hello same port in hello VM’s firewall.</span></span> <span data-ttu-id="ef6c1-346">koncový bod Hello byl vytvořen při zřizování hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-346">hello endpoint was created when hello VM was provisioned.</span></span>

<span data-ttu-id="ef6c1-347">Tato část obsahuje základní informace o tom, jak tooopen hello port brány firewall.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-347">This section provides basic information on how tooopen hello firewall port.</span></span> <span data-ttu-id="ef6c1-348">Další informace najdete v tématu [Configure a Firewall for Report Server Access](https://technet.microsoft.com/library/bb934283.aspx)</span><span class="sxs-lookup"><span data-stu-id="ef6c1-348">For more information, see [Configure a Firewall for Report Server Access](https://technet.microsoft.com/library/bb934283.aspx)</span></span>

> [!NOTE]
> <span data-ttu-id="ef6c1-349">Pokud jste použili hello skriptu tooconfigure hello sestavy serveru, můžete tuto část přeskočit.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-349">If you used hello script tooconfigure hello report server, you can skip this section.</span></span> <span data-ttu-id="ef6c1-350">skript Hello zahrnuty port brány firewall krok tooopen hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-350">hello script included a step tooopen hello firewall port.</span></span>
> 
> 

<span data-ttu-id="ef6c1-351">Pokud jste nakonfigurovali privátní port pro protokol HTTPS 443, upravte následující skript správně hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-351">If you configured a private port for HTTPS other than 443, modify hello following script appropriately.</span></span> <span data-ttu-id="ef6c1-352">tooopen port **443** na hello brány Windows Firewall, dokončete následující hello:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-352">tooopen port **443** on hello Windows Firewall, complete hello following:</span></span>

1. <span data-ttu-id="ef6c1-353">Otevřete okno prostředí Windows PowerShell s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-353">Open a Windows PowerShell window with administrative privileges.</span></span>
2. <span data-ttu-id="ef6c1-354">Pokud jste použili jiný port než 443, když jste nakonfigurovali koncový bod HTTPS hello na hello virtuálních počítačů, aktualizujte hello port v hello následující příkaz a poté spusťte příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-354">If you used a port other than 443 when you configured hello HTTPS endpoint on hello VM, update hello port in hello following command and then run hello command:</span></span>
   
        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443
3. <span data-ttu-id="ef6c1-355">Po dokončení příkazu hello **Ok** se zobrazí v hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-355">When hello command completes, **Ok** is displayed in hello command prompt.</span></span>

<span data-ttu-id="ef6c1-356">tooverify, že je otevřen hello port, otevřete okno prostředí Windows PowerShell a spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-356">tooverify that hello port is opened, open a Windows PowerShell window and run hello following command:</span></span>

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-hello-configuration"></a><span data-ttu-id="ef6c1-357">Ověření konfigurace hello</span><span class="sxs-lookup"><span data-stu-id="ef6c1-357">Verify hello configuration</span></span>
<span data-ttu-id="ef6c1-358">tooverify, který nyní pracuje funkce serveru hello základní sestavy, otevřete prohlížeč s oprávněními správce a pak procházet toohello následující zprávy Správce sestav služby ad serveru adresy URL:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-358">tooverify that hello basic report server functionality is now working, open your browser with administrative privileges and then browse toohello following report server ad report manager URLS:</span></span>

* <span data-ttu-id="ef6c1-359">Na hello virtuálních počítačů procházet toohello adresa URL serveru sestav:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-359">On hello VM, browse toohello report server URL:</span></span>
  
        http://localhost/reportserver
* <span data-ttu-id="ef6c1-360">Na hello virtuálních počítačů procházet toohello adresa URL správce sestav:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-360">On hello VM, browse toohello report manger URL:</span></span>
  
        http://localhost/Reports
* <span data-ttu-id="ef6c1-361">Z místního počítače, vyhledejte toohello **vzdáleného** Manager vykazovat hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-361">From your local computer, browse toohello **remote** report Manager on hello VM.</span></span> <span data-ttu-id="ef6c1-362">Název DNS hello v hello následující ukázka podle potřeby aktualizujte.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-362">Update hello DNS name in hello following example as appropriate.</span></span> <span data-ttu-id="ef6c1-363">Po zobrazení výzvy k zadání hesla, použijte přihlašovací údaje správce hello, kterou jste vytvořili při zřizování hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-363">When prompted for a password, use hello administrator credentials you created when hello VM was provisioned.</span></span> <span data-ttu-id="ef6c1-364">Hello uživatelské jméno je v hello [doména]\[uživatelské jméno] formátu, kde doména hello je název počítače hello virtuálních počítačů, například ssrsnativecloud\testuser.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-364">hello user name is in hello [Domain]\[user name] format, where hello domain is hello VM computer name, for example ssrsnativecloud\testuser.</span></span> <span data-ttu-id="ef6c1-365">Pokud nepoužíváte HTTP**S**, odeberte hello **s** v adrese URL hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-365">If you are not using HTTP**S**, remove hello **s** in hello URL.</span></span> <span data-ttu-id="ef6c1-366">V části hello Další informace o vytvoření dalších uživatelů na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-366">See hello next section for information on creating additional users on VM.</span></span>
  
        https://ssrsnativecloud.cloudapp.net/Reports
* <span data-ttu-id="ef6c1-367">Z místního počítače vyhledejte adresa URL toohello sestavu vzdáleného serveru.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-367">From your local computer, browse toohello remote report server URL.</span></span> <span data-ttu-id="ef6c1-368">Název DNS hello v hello následující ukázka podle potřeby aktualizujte.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-368">Update hello DNS name in hello following example as appropriate.</span></span> <span data-ttu-id="ef6c1-369">Pokud nepoužíváte protokol HTTPS, odeberte hello s v adrese URL hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-369">If you are not using HTTPS, remove hello s in hello URL.</span></span>
  
        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a><span data-ttu-id="ef6c1-370">Vytvoření uživatelů a přiřazování rolí</span><span class="sxs-lookup"><span data-stu-id="ef6c1-370">Create Users and Assign Roles</span></span>
<span data-ttu-id="ef6c1-371">Konfigurace a ověření hello sestavu serveru běžné úlohy správy je toocreate jeden nebo více uživatelů a přiřazení uživatelů tooReporting služby rolí.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-371">After configuring and verifying hello report server, a common administrative task is toocreate one or more users and assign users tooReporting Services roles.</span></span> <span data-ttu-id="ef6c1-372">Další informace najdete v tématu hello následující:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-372">For more information, see hello following:</span></span>

* [<span data-ttu-id="ef6c1-373">Vytvořte místní uživatelský účet</span><span class="sxs-lookup"><span data-stu-id="ef6c1-373">Create a local user account</span></span>](https://technet.microsoft.com/library/cc770642.aspx)
* <span data-ttu-id="ef6c1-374">[Udělení přístupu uživatele tooa serveru sestav (Správce sestav)](https://msdn.microsoft.com/library/ms156034.aspx))</span><span class="sxs-lookup"><span data-stu-id="ef6c1-374">[Grant User Access tooa Report Server (Report Manager)](https://msdn.microsoft.com/library/ms156034.aspx))</span></span>
* [<span data-ttu-id="ef6c1-375">Vytvoření a správa přiřazení rolí</span><span class="sxs-lookup"><span data-stu-id="ef6c1-375">Create and Manage Role Assignments</span></span>](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="toocreate-and-publish-reports-toohello-azure-virtual-machine"></a><span data-ttu-id="ef6c1-376">tooCreate a publikovat sestavy toohello virtuální počítač Azure</span><span class="sxs-lookup"><span data-stu-id="ef6c1-376">tooCreate and Publish Reports toohello Azure Virtual Machine</span></span>
<span data-ttu-id="ef6c1-377">Hello následující tabulka shrnuje některé hello možnosti k dispozici toopublish existující sestavy ze serveru místní počítač toohello sestavy musí být hostované na hello virtuální počítač Microsoft Azure:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-377">hello following table summarizes some of hello options available toopublish existing reports from an on-premises computer toohello report server hosted on hello Microsoft Azure Virtual Machine:</span></span>

* <span data-ttu-id="ef6c1-378">**Skript RS.exe**: položky sestavy toocopy RS.exe použití skriptu z a existující sestavy serveru tooyour virtuální počítač Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-378">**RS.exe script**: Use RS.exe script toocopy report items from and existing report server tooyour Microsoft Azure Virtual Machine.</span></span> <span data-ttu-id="ef6c1-379">Další informace naleznete v části hello "nativní režim tooNative režimu – virtuálním počítači Microsoft Azure" [ukázka Reporting Services rs.exe skriptu tooMigrate obsah mezi servery sestav](https://msdn.microsoft.com/library/dn531017.aspx).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-379">For more information, see hello section “Native mode tooNative Mode – Microsoft Azure Virtual Machine” in [Sample Reporting Services rs.exe Script tooMigrate Content between Report Servers](https://msdn.microsoft.com/library/dn531017.aspx).</span></span>
* <span data-ttu-id="ef6c1-380">**Tvůrce sestav**: hello virtuální počítač obsahuje hello kliknutím-jednou verzi Tvůrce sestav Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-380">**Report Builder**: hello virtual machine includes hello click-once version of Microsoft SQL Server Report Builder.</span></span> <span data-ttu-id="ef6c1-381">toostart sestavy Tvůrce hello poprvé hello virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-381">toostart Report builder hello first time on hello virtual machine:</span></span>
  
  1. <span data-ttu-id="ef6c1-382">Spustíte prohlížeč s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-382">Start your browser with administrative privileges.</span></span>
  2. <span data-ttu-id="ef6c1-383">Procházet tooreport manager hello virtuálního počítače a klikněte na tlačítko **Tvůrce sestav** hello pásu karet.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-383">Browse tooreport manager on hello virtual machine and click **Report Builder**  in hello ribbon.</span></span>
     
     <span data-ttu-id="ef6c1-384">Další informace najdete v tématu [instalace, odinstalace a podpora Tvůrce sestav](https://technet.microsoft.com/library/dd207038.aspx).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-384">For more information, see [Installing, Uninstalling, and Supporting Report Builder](https://technet.microsoft.com/library/dd207038.aspx).</span></span>
* <span data-ttu-id="ef6c1-385">**SQL Server Data Tools: Virtuální počítač**: Pokud jste vytvořili hello virtuálního počítače s SQL serverem 2012, pak SQL Server Data Tools je nainstalovaná hello virtuálního počítače a může být použité toocreate **projekty serveru sestav** a sestavy na virtuálním hello počítač.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-385">**SQL Server Data Tools: VM**:  If you created hello VM with SQL Server 2012, then SQL Server Data Tools is installed on hello virtual machine and can be used toocreate **Report Server Projects** and reports on hello virtual machine.</span></span> <span data-ttu-id="ef6c1-386">SQL Server Data Tools můžete publikovat hello sestavy toohello serveru sestav na virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-386">SQL Server Data Tools can publish hello reports toohello report server on hello virtual machine.</span></span>
  
    <span data-ttu-id="ef6c1-387">Pokud jste vytvořili hello virtuálních počítačů se systémem SQL server 2014, můžete nainstalovat SQL Server Data Tools - BI pro sadu visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-387">If you created hello VM with SQL server 2014, you can install SQL Server Data Tools- BI for visual Studio.</span></span> <span data-ttu-id="ef6c1-388">Další informace najdete v tématu hello následující:</span><span class="sxs-lookup"><span data-stu-id="ef6c1-388">For more information, see hello following:</span></span>
  
  * [<span data-ttu-id="ef6c1-389">Nástroje Microsoft SQL Server Data Tools – Business Intelligence pro Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="ef6c1-389">Microsoft SQL Server Data Tools - Business Intelligence for Visual Studio 2013</span></span>](https://www.microsoft.com/download/details.aspx?id=42313)
  * [<span data-ttu-id="ef6c1-390">Nástroje Microsoft SQL Server Data Tools – Business Intelligence pro sadu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="ef6c1-390">Microsoft SQL Server Data Tools - Business Intelligence for Visual Studio 2012</span></span>](https://www.microsoft.com/download/details.aspx?id=36843)
  * [<span data-ttu-id="ef6c1-391">Nástroje SQL Server Data Tools a SQL Server Business Intelligence (SSDT-BI)</span><span class="sxs-lookup"><span data-stu-id="ef6c1-391">SQL Server Data Tools and SQL Server Business Intelligence (SSDT-BI)</span></span>](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)
* <span data-ttu-id="ef6c1-392">**SQL Server Data Tools: Vzdálené**: V místním počítači, vytvoření projektu služby Reporting Services v SQL Server Data Tools, obsahující sestavy služby Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-392">**SQL Server Data Tools: Remote**:  On your local computer, create a Reporting Services project in SQL Server Data Tools that contains Reporting Services reports.</span></span> <span data-ttu-id="ef6c1-393">Nakonfigurujte hello projektu tooconnect toohello adresa URL webové služby.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-393">Configure hello project tooconnect toohello web service URL.</span></span>
  
    ![Vlastnosti projektu rozšíření SSDT pro projekt služby SSRS](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)
* <span data-ttu-id="ef6c1-395">**Použít skript**: použijte skript toocopy sestavy serveru obsahu.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-395">**Use script**: Use script toocopy report server content.</span></span> <span data-ttu-id="ef6c1-396">Další informace najdete v tématu [ukázka Reporting Services rs.exe skriptu tooMigrate obsah mezi servery sestav](https://msdn.microsoft.com/library/dn531017.aspx).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-396">For more information, see [Sample Reporting Services rs.exe Script tooMigrate Content between Report Servers](https://msdn.microsoft.com/library/dn531017.aspx).</span></span>

## <a name="minimize-cost-if-you-are-not-using-hello-vm"></a><span data-ttu-id="ef6c1-397">Minimalizovat náklady, pokud nepoužíváte hello virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="ef6c1-397">Minimize cost if you are not using hello VM</span></span>
> [!NOTE]
> <span data-ttu-id="ef6c1-398">poplatky toominimize pro virtuální počítače Azure při není používán, vypněte hello virtuálních počítačů z hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-398">toominimize charges for your Azure Virtual Machines when not in use, shut down hello VM from hello Azure classic portal.</span></span> <span data-ttu-id="ef6c1-399">Pokud použijete možnosti napájení Windows hello uvnitř virtuálního počítače tooshut dolů hello virtuálních počítačů, které budou účtovat hello stejné částka pro hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-399">If you use hello Windows power options inside a VM tooshut down hello VM, you are still charged hello same amount for hello VM.</span></span> <span data-ttu-id="ef6c1-400">tooreduce poplatky, budete potřebovat tooshut dolů hello virtuálních počítačů v hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="ef6c1-400">tooreduce charges, you need tooshut down hello VM in hello Azure classic portal.</span></span> <span data-ttu-id="ef6c1-401">Pokud již nepotřebujete hello virtuálních počítačů, mějte na paměti, toodelete hello virtuálních počítačů a hello přidružené VHD poplatky za uložení tooavoid soubory. Další informace najdete v tématu oddíl hello – nejčastější dotazy na [podrobnosti o cenách virtuálních počítačů](https://azure.microsoft.com/pricing/details/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-401">If you no longer need hello VM, remember toodelete hello VM and hello associated .vhd files tooavoid storage charges.For more information, see hello FAQ section at [Virtual Machines Pricing Details](https://azure.microsoft.com/pricing/details/virtual-machines/).</span></span>

## <a name="more-information"></a><span data-ttu-id="ef6c1-402">Další informace</span><span class="sxs-lookup"><span data-stu-id="ef6c1-402">More Information</span></span>
### <a name="resources"></a><span data-ttu-id="ef6c1-403">Zdroje</span><span class="sxs-lookup"><span data-stu-id="ef6c1-403">Resources</span></span>
* <span data-ttu-id="ef6c1-404">Podobně jako obsah související s tooa nasazení jednoho serveru SQL Server Business Intelligence a SharePoint 2013, najdete v části [tooCreate použijte rozhraní Windows PowerShell Azure virtuálního počítače s SQL Server BI a SharePoint 2013](https://msdn.microsoft.com/library/azure/dn385843.aspx).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-404">For similar content related tooa single server deployment of SQL Server Business Intelligence and SharePoint 2013, see [Use Windows PowerShell tooCreate an Azure VM With SQL Server BI and SharePoint 2013](https://msdn.microsoft.com/library/azure/dn385843.aspx).</span></span>
* <span data-ttu-id="ef6c1-405">Podobně jako obsah související tooa nasazení více serverů SQL Server Business Intelligence a SharePoint 2013, najdete v části [nasazení SQL Server Business Intelligence v Azure Virtual Machines](https://msdn.microsoft.com/library/dn321998.aspx).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-405">For similar content related tooa multi-server deployment of SQL Server Business Intelligence and SharePoint 2013, see [Deploy SQL Server Business Intelligence in Azure Virtual Machines](https://msdn.microsoft.com/library/dn321998.aspx).</span></span>
* <span data-ttu-id="ef6c1-406">Obecné informace najdete v části související toodeployments SQL Server Business Intelligence v Azure Virtual Machines [SQL Server Business Intelligence v Azure Virtual Machines](virtual-machines-windows-classic-ps-sql-bi.md).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-406">For General information related toodeployments of SQL Server Business Intelligence in Azure Virtual Machines, see [SQL Server Business Intelligence in Azure Virtual Machines](virtual-machines-windows-classic-ps-sql-bi.md).</span></span>
* <span data-ttu-id="ef6c1-407">Další informace o hello náklady poplatky za výpočty Azure najdete v tématu kartě virtuální počítače hello [Azure cenové kalkulačky](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-407">For more information about hello cost of Azure compute charges, see hello Virtual Machines tab of [Azure pricing calculator](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).</span></span>

### <a name="community-content"></a><span data-ttu-id="ef6c1-408">Komunitním obsahu</span><span class="sxs-lookup"><span data-stu-id="ef6c1-408">Community Content</span></span>
* <span data-ttu-id="ef6c1-409">Pokyny krok za krokem jak toocreate Reporting Services na nativní režim serveru sestav bez použití skriptu, najdete v části [hostování služby SQL Reporting Services na virtuální počítač Azure](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).</span><span class="sxs-lookup"><span data-stu-id="ef6c1-409">For step by step instructions on how toocreate a Reporting Services Native mode report server without using script, see [Hosting SQL Reporting Service on Azure Virtual Machine](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).</span></span>

### <a name="links-tooother-resources-for-sql-server-in-azure-vms"></a><span data-ttu-id="ef6c1-410">Odkazy tooother prostředky pro SQL Server ve virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="ef6c1-410">Links tooother resources for SQL Server in Azure VMs</span></span>
[<span data-ttu-id="ef6c1-411">SQL Server na virtuálních počítačích Azure – přehled</span><span class="sxs-lookup"><span data-stu-id="ef6c1-411">SQL Server on Azure Virtual Machines Overview</span></span>](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

