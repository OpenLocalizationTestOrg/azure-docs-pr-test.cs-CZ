1. <span data-ttu-id="1db1d-101">Kliknutím na **Připojit** vytvoříte a stáhnete soubor protokolu Remote Desktop Protocol (.rdp).</span><span class="sxs-lookup"><span data-stu-id="1db1d-101">Clicking **Connect** creates and downloads a Remote Desktop Protocol file (.rdp file).</span></span> <span data-ttu-id="1db1d-102">Kliknutím na **Otevřít** tento soubor použijete.</span><span class="sxs-lookup"><span data-stu-id="1db1d-102">Click **Open** to use this file.</span></span>
2. <span data-ttu-id="1db1d-103">Zobrazí se upozornění, že soubor RDP je od neznámého vydavatele.</span><span class="sxs-lookup"><span data-stu-id="1db1d-103">You will get a warning that the .rdp is from an unknown publisher.</span></span> <span data-ttu-id="1db1d-104">To je normální.</span><span class="sxs-lookup"><span data-stu-id="1db1d-104">This is normal.</span></span> <span data-ttu-id="1db1d-105">Pokračujte kliknutím na **Připojit** v okně Připojení ke vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="1db1d-105">In the Remote Desktop window, click **Connect** to continue.</span></span>
   
    ![Snímek obrazovky upozornění na neznámého vydavatele](./media/virtual-machines-log-on-win-server/rdp-warn.png)
3. <span data-ttu-id="1db1d-107">V okně **Zabezpečení systému Windows** zadejte přihlašovací údaje k účtu virtuálního počítače a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="1db1d-107">In the **Windows Security** window, type the credentials for an account on the virtual machine and then click **OK**.</span></span>
   
     <span data-ttu-id="1db1d-108">**Místní účet** – Většinou je to uživatelské jméno a heslo k místnímu účtu, které jste zadávali při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1db1d-108">**Local account** - this is usually the local account user name and password that you specified when you created the virtual machine.</span></span> <span data-ttu-id="1db1d-109">V tomto případě je doménou název virtuálního počítače ve formátu *název_virtuálního_počítače*&#92;*uživatelské_jméno*.</span><span class="sxs-lookup"><span data-stu-id="1db1d-109">In this case, the domain is the name of the virtual machine and it is entered as *vmname*&#92;*username*.</span></span>  
   
    <span data-ttu-id="1db1d-110">**Virtuální počítač připojený k doméně** – Pokud virtuální počítač patří do domény, zadejte uživatelské jméno ve formátu *doména*&#92;*uživatelské_jméno*.</span><span class="sxs-lookup"><span data-stu-id="1db1d-110">**Domain joined VM** - if the VM belongs to a domain, enter the user name in the format *Domain*&#92;*Username*.</span></span> <span data-ttu-id="1db1d-111">Účet také musí být členem skupiny Administrators nebo musí mít udělené oprávnění ke vzdálenému přístupu k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="1db1d-111">The account also needs to either be in the Administrators group or have been granted remote access privileges to the VM.</span></span>
   
    <span data-ttu-id="1db1d-112">**Řadič domény** – Pokud je virtuální počítač řadičem domény, zadejte uživatelské jméno a heslo účtu správce domény pro danou doménu.</span><span class="sxs-lookup"><span data-stu-id="1db1d-112">**Domain controller** - if the VM is a domain controller, type the user name and password of a domain administrator account for that domain.</span></span>
4. <span data-ttu-id="1db1d-113">Kliknutím na **Ano** ověřte identitu virtuálního počítače a dokončete přihlášení.</span><span class="sxs-lookup"><span data-stu-id="1db1d-113">Click **Yes** to verify the identity of the virtual machine and finish logging on.</span></span>
   
   ![Snímek obrazovky zobrazující zprávu o ověření identity virtuálního počítače](./media/virtual-machines-log-on-win-server/cert-warning.png)
