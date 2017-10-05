---
title: "Azure ukázkovou aplikaci pro použití s zóny DMZ | Microsoft Docs"
description: "Nasazení této jednoduché webové aplikace po vytvoření DMZ k otestování scénářů tok provozu"
services: virtual-network
documentationcenter: na
author: tracsman
manager: rossort
editor: 
ms.assetid: 60340ab7-b82b-40e0-bd87-83e41fe4519c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jonor
ms.openlocfilehash: 8506238e41c5d9dac8d76d729d4919b30a0528b9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="sample-application-for-use-with-dmzs"></a><span data-ttu-id="8a4fd-103">Ukázková aplikace pro použití s zóny DMZ</span><span class="sxs-lookup"><span data-stu-id="8a4fd-103">Sample application for use with DMZs</span></span>
<span data-ttu-id="8a4fd-104">[Návrat na stránku osvědčené postupy zabezpečení hranic][HOME]</span><span class="sxs-lookup"><span data-stu-id="8a4fd-104">[Return to the Security Boundary Best Practices Page][HOME]</span></span>

<span data-ttu-id="8a4fd-105">Tyto skripty prostředí PowerShell můžete spustit místně na serveru IIS01 a AppVM01 nainstalovat a nastavit jednoduché webové aplikace, který zobrazí stránku html z front-endu IIS01 server s obsahem ze serveru back-end AppVM01.</span><span class="sxs-lookup"><span data-stu-id="8a4fd-105">These PowerShell scripts can be run locally on the IIS01 and AppVM01 servers to install and set up a simple web application that displays an html page from the front-end IIS01 server with content from the back-end AppVM01 server.</span></span>

<span data-ttu-id="8a4fd-106">Tuto aplikaci poskytuje jednoduché testovacím prostředí pro mnoho příkladů DMZ a jak změny na koncové body, skupiny Nsg, UDR a brány Firewall pravidla může ovlivnit tok provozu.</span><span class="sxs-lookup"><span data-stu-id="8a4fd-106">This application provides a simple testing environment for many of the DMZ Examples and how changes on the Endpoints, NSGs, UDR, and Firewall rules can affect traffic flows.</span></span>

## <a name="firewall-rule-to-allow-icmp"></a><span data-ttu-id="8a4fd-107">Pravidlo brány firewall, které povolí protokol ICMP</span><span class="sxs-lookup"><span data-stu-id="8a4fd-107">Firewall rule to allow ICMP</span></span>
<span data-ttu-id="8a4fd-108">Tento jednoduchý příkaz prostředí PowerShell můžete spustit na jakékoli virtuální počítač s Windows povolit provoz protokolu ICMP (Ping).</span><span class="sxs-lookup"><span data-stu-id="8a4fd-108">This simple PowerShell statement can be run on any Windows VM to allow ICMP (Ping) traffic.</span></span> <span data-ttu-id="8a4fd-109">Tato aktualizace brány firewall umožňuje snazší testování a řešení potíží s tím, že protokol ping přes bránu firewall systému windows (pro většinu distribucích systému Linux, ve které ICMP ve výchozím nastavení).</span><span class="sxs-lookup"><span data-stu-id="8a4fd-109">This firewall update allows for easier testing and troubleshooting by allowing the ping protocol to pass through the windows firewall (for most Linux distros ICMP is on by default).</span></span>

```PowerShell
# Turn On ICMPv4
New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" `
    -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
```

<span data-ttu-id="8a4fd-110">Pokud chcete použít následující skripty, přidání pravidla brány firewall je první příkaz.</span><span class="sxs-lookup"><span data-stu-id="8a4fd-110">If you use the following scripts, this firewall rule addition is the first statement.</span></span>

## <a name="iis01---web-application-installation-script"></a><span data-ttu-id="8a4fd-111">IIS01 - skript instalace webové aplikace</span><span class="sxs-lookup"><span data-stu-id="8a4fd-111">IIS01 - Web application installation script</span></span>
<span data-ttu-id="8a4fd-112">Bude se tento skript:</span><span class="sxs-lookup"><span data-stu-id="8a4fd-112">This script will:</span></span>

1. <span data-ttu-id="8a4fd-113">Otevřete IMCPv4 (Ping) na místním serveru brány windows firewall pro snazší testování</span><span class="sxs-lookup"><span data-stu-id="8a4fd-113">Open IMCPv4 (Ping) on the local server windows firewall for easier testing</span></span>
2. <span data-ttu-id="8a4fd-114">Instalace IIS a .net Framework v4.5</span><span class="sxs-lookup"><span data-stu-id="8a4fd-114">Install IIS and the .Net Framework v4.5</span></span>
3. <span data-ttu-id="8a4fd-115">Vytvořit webovou stránku ASP.NET a v souboru Web.config</span><span class="sxs-lookup"><span data-stu-id="8a4fd-115">Create an ASP.NET web page and a Web.config file</span></span>
4. <span data-ttu-id="8a4fd-116">Změna fondu aplikací výchozí usnadnění přístupu k souborům</span><span class="sxs-lookup"><span data-stu-id="8a4fd-116">Change the Default application pool to make file access easier</span></span>
5. <span data-ttu-id="8a4fd-117">Nastavit pomocí účtu anonymního uživatele na váš účet správce a heslo</span><span class="sxs-lookup"><span data-stu-id="8a4fd-117">Set the Anonymous user to your admin account and password</span></span>

<span data-ttu-id="8a4fd-118">Tento skript prostředí PowerShell by měl být spuštěn místně při RDP měl do IIS01.</span><span class="sxs-lookup"><span data-stu-id="8a4fd-118">This PowerShell script should be run locally while RDP’d into IIS01.</span></span>

```PowerShell
# IIS Server Post Build Config Script
# Get Admin Account and Password
    Write-Host "Please enter the admin account information used to create this VM:" -ForegroundColor Cyan
    $theAdmin = Read-Host -Prompt "The Admin Account Name (no domain or machine name)"
    $thePassword = Read-Host -Prompt "The Admin Password"

# Turn On ICMPv4
    Write-Host "Creating ICMP Rule in Windows Firewall" -ForegroundColor Cyan
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

# Install IIS
    Write-Host "Installing IIS and .Net 4.5, this can take some time, like 15+ minutes..." -ForegroundColor Cyan
    add-windowsfeature Web-Server, Web-WebServer, Web-Common-Http, Web-Default-Doc, Web-Dir-Browsing, Web-Http-Errors, Web-Static-Content, Web-Health, Web-Http-Logging, Web-Performance, Web-Stat-Compression, Web-Security, Web-Filtering, Web-App-Dev, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Net-Ext, Web-Net-Ext45, Web-Asp-Net45, Web-Mgmt-Tools, Web-Mgmt-Console

# Create Web App Pages
    Write-Host "Creating Web page and Web.Config file" -ForegroundColor Cyan
    $MainPage = '<%@ Page Language="vb" AutoEventWireup="false" %>
    <%@ Import Namespace="System.IO" %>
    <script language="vb" runat="server">
        Protected Sub Page_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Load
            Dim FILENAME As String = "\\10.0.2.5\WebShare\Rand.txt"
            Dim objStreamReader As StreamReader
            objStreamReader = File.OpenText(FILENAME)
            Dim contents As String = objStreamReader.ReadToEnd()
            lblOutput.Text = contents
            objStreamReader.Close()
            lblTime.Text = Now()
        End Sub
    </script>

    <!DOCTYPE html>
    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
        <title>DMZ Example App</title>
    </head>
    <body style="font-family: Optima,Segoe,Segoe UI,Candara,Calibri,Arial,sans-serif;">
      <form id="frmMain" runat="server">
        <div>
          <h1>Looks like you made it!</h1>
          This is a page from the inside (a web server on a private network),<br />
          and it is making its way to the outside! (If you are viewing this from the internet)<br />
          <br />
          The following sections show:
          <ul style="margin-top: 0px;">
            <li> Local Server Time - Shows if this page is or isnt cached anywhere</li>
            <li> File Output - Shows that the web server is reaching AppVM01 on the backend subnet and successfully returning content</li>
            <li> Image from the Internet - Doesnt really show anything, but it made me happy to see this when the app worked</li>
          </ul>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>Local Web Server Time</b>: <asp:Label runat="server" ID="lblTime" /></div>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>File Output from AppVM01</b>: <asp:Label runat="server" ID="lblOutput" /></div>
          <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
            <b>Image File Linked from the Internet</b>:<br />
            <br />
            <img src="http://sd.keepcalm-o-matic.co.uk/i/keep-calm-you-made-it-7.png" alt="You made it!" width="150" length="175"/></div>
        </div>
      </form>
    </body>
    </html>'

    $WebConfig ='<?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.web>
        <compilation debug="true" strict="false" explicit="true" targetFramework="4.5" />
        <httpRuntime targetFramework="4.5" />
        <identity impersonate="true" />
        <customErrors mode="Off"/>
      </system.web>
      <system.webServer>
        <defaultDocument>
          <files>
            <add value="Home.aspx" />
          </files>
        </defaultDocument>
      </system.webServer>
    </configuration>'

    $MainPage | Out-File -FilePath "C:\inetpub\wwwroot\Home.aspx" -Encoding ascii
    $WebConfig | Out-File -FilePath "C:\inetpub\wwwroot\Web.config" -Encoding ascii

# Set App Pool to Clasic Pipeline to remote file access will work easier
    Write-Host "Updaing IIS Settings" -ForegroundColor Cyan
    c:\windows\system32\inetsrv\appcmd.exe set app "Default Web Site/" /applicationPool:".NET v4.5 Classic"
    c:\windows\system32\inetsrv\appcmd.exe set config "Default Web Site/" /section:system.webServer/security/authentication/anonymousAuthentication /userName:$theAdmin /password:$thePassword /commit:apphost

# Make sure the IIS settings take
    Write-Host "Restarting the W3SVC" -ForegroundColor Cyan
    Restart-Service -Name W3SVC

    Write-Host
    Write-Host "Web App Creation Successfull!" -ForegroundColor Green
    Write-Host
```

## <a name="appvm01---file-server-installation-script"></a><span data-ttu-id="8a4fd-119">AppVM01 - soubor skriptu instalace serveru</span><span class="sxs-lookup"><span data-stu-id="8a4fd-119">AppVM01 - File server installation script</span></span>
<span data-ttu-id="8a4fd-120">Tento skript nastaví back-end pro tuto aplikaci jednoduché.</span><span class="sxs-lookup"><span data-stu-id="8a4fd-120">This script sets up the back-end for this simple application.</span></span> <span data-ttu-id="8a4fd-121">Bude se tento skript:</span><span class="sxs-lookup"><span data-stu-id="8a4fd-121">This script will:</span></span>

1. <span data-ttu-id="8a4fd-122">Otevřete IMCPv4 (Ping) v bráně firewall pro snazší testování</span><span class="sxs-lookup"><span data-stu-id="8a4fd-122">Open IMCPv4 (Ping) on the firewall for easier testing</span></span>
2. <span data-ttu-id="8a4fd-123">Vytvořte adresář pro web</span><span class="sxs-lookup"><span data-stu-id="8a4fd-123">Create a directory for the web site</span></span>
3. <span data-ttu-id="8a4fd-124">Vytvořte textový soubor být vzdáleně přístup webové stránky</span><span class="sxs-lookup"><span data-stu-id="8a4fd-124">Create a text file to be remotely access by the web page</span></span>
4. <span data-ttu-id="8a4fd-125">Nastavení oprávnění pro adresáře a souboru pro anonymní přístup</span><span class="sxs-lookup"><span data-stu-id="8a4fd-125">Set permissions on the directory and file to Anonymous to allow access</span></span>
5. <span data-ttu-id="8a4fd-126">Vypněte rozšířené zabezpečení Internet Exploreru umožňuje snazší procházení z tohoto serveru</span><span class="sxs-lookup"><span data-stu-id="8a4fd-126">Turn off IE Enhanced Security to allow easier browsing from this server</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="8a4fd-127">**Nejvhodnější**: nikdy vypněte rozšířené zabezpečení Internet Exploreru na provozním serveru a obecně je vhodné procházení webu, z provozním serveru.</span><span class="sxs-lookup"><span data-stu-id="8a4fd-127">**Best Practice**: Never turn off IE Enhanced Security on a production server, plus it's generally a bad idea to surf the web from a production server.</span></span> <span data-ttu-id="8a4fd-128">Otevírání do sdílené složky pro anonymní přístup je také vhodné, ale done sem pro jednoduchost.</span><span class="sxs-lookup"><span data-stu-id="8a4fd-128">Also, opening up file shares for anonymous access is a bad idea, but done here for simplicity.</span></span>
> 
> 

<span data-ttu-id="8a4fd-129">Tento skript prostředí PowerShell by měl být spuštěn místně při RDP měl do AppVM01.</span><span class="sxs-lookup"><span data-stu-id="8a4fd-129">This PowerShell script should be run locally while RDP’d into AppVM01.</span></span> <span data-ttu-id="8a4fd-130">PowerShell je nutné spustit jako správce k zajištění úspěšné provedení.</span><span class="sxs-lookup"><span data-stu-id="8a4fd-130">PowerShell is required to be run as Administrator to ensure successful execution.</span></span>

```PowerShell
# AppVM01 Server Post Build Config Script
# PowerShell must be run as Administrator for Net Share commands to work

# Turn On ICMPv4
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

# Create Directory
    New-Item "C:\WebShare" -ItemType Directory

# Write out Rand.txt
    $FileContent = "Hello, I'm the contents of a remote file on AppVM01."
    $FileContent | Out-File -FilePath "C:\WebShare\Rand.txt" -Encoding ascii

# Set Permissions on share
    $Acl = Get-Acl "C:\WebShare"
    $AccessRule = New-Object system.security.accesscontrol.filesystemaccessrule("Everyone","ReadAndExecute, Synchronize","ContainerInherit, ObjectInherit","InheritOnly","Allow")
    $Acl.SetAccessRule($AccessRule)
    Set-Acl "C:\WebShare" $Acl

# Create network share
    Net Share WebShare=C:\WebShare "/grant:Everyone,READ"

# Turn Off IE Enhanced Security Configuration for Admins
    Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0

    Write-Host
    Write-Host "File Server Set up Successfull!" -ForegroundColor Green
    Write-Host
```

## <a name="dns01---dns-server-installation-script"></a><span data-ttu-id="8a4fd-131">DNS01 - skript instalace serveru DNS</span><span class="sxs-lookup"><span data-stu-id="8a4fd-131">DNS01 - DNS server installation script</span></span>
<span data-ttu-id="8a4fd-132">Neexistuje žádný skript zahrnuté v této ukázkové aplikaci nastavení serveru DNS.</span><span class="sxs-lookup"><span data-stu-id="8a4fd-132">There is no script included in this sample application to set up the DNS server.</span></span> <span data-ttu-id="8a4fd-133">Pokud testování pravidel brány firewall, NSG nebo UDR musí obsahovat přenosy DNS, DNS01 server musí ručně nastavit.</span><span class="sxs-lookup"><span data-stu-id="8a4fd-133">If testing of the firewall rules, NSG, or UDR needs to include DNS traffic, the DNS01 server needs to be set up manually.</span></span> <span data-ttu-id="8a4fd-134">Soubor xml konfigurace sítě a šablonou Resource Manageru pro oba příklady zahrnuje DNS01 jako primární server DNS a veřejný server DNS hostitelem úroveň 3 jako záložní server DNS.</span><span class="sxs-lookup"><span data-stu-id="8a4fd-134">The Network Configuration xml file and Resource Manager Template for both examples includes DNS01 as the primary DNS server and the public DNS server hosted by Level 3 as the backup DNS server.</span></span> <span data-ttu-id="8a4fd-135">Server DNS 3 úrovně bude server DNS použitý pro jiné než místní provoz a DNS01 není nastavit, v místní síti, který by tomu bylo DNS.</span><span class="sxs-lookup"><span data-stu-id="8a4fd-135">The Level 3 DNS server would be the actual DNS server used for non-local traffic, and with DNS01 not setup, no local network DNS would occur.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8a4fd-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8a4fd-136">Next steps</span></span>
* <span data-ttu-id="8a4fd-137">Spuštění skriptu IIS01 na server služby IIS</span><span class="sxs-lookup"><span data-stu-id="8a4fd-137">Run the IIS01 script on an IIS server</span></span>
* <span data-ttu-id="8a4fd-138">Spusťte skript souborového serveru na AppVM01</span><span class="sxs-lookup"><span data-stu-id="8a4fd-138">Run File Server script on AppVM01</span></span>
* <span data-ttu-id="8a4fd-139">Přejděte do veřejné IP adresy na IIS01 k ověření vaší sestavení</span><span class="sxs-lookup"><span data-stu-id="8a4fd-139">Browse to the Public IP on IIS01 to validate your build</span></span>

<!--Link References-->
[HOME]: ../best-practices-network-security.md
