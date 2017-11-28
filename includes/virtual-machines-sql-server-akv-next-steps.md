## <a name="next-steps"></a><span data-ttu-id="fb71a-101">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fb71a-101">Next steps</span></span>

<span data-ttu-id="fb71a-102">Po povolení Azure Key Vault integrace, můžete povolit šifrování SQL serveru na virtuální počítač SQL.</span><span class="sxs-lookup"><span data-stu-id="fb71a-102">After enabling Azure Key Vault Integration, you can enable SQL Server encryption on your SQL VM.</span></span> <span data-ttu-id="fb71a-103">Nejprve musíte vytvořit asymetrický klíč v trezoru klíčů a symetrický klíč v rámci systému SQL Server na vašem virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="fb71a-103">First, you will need to create an asymmetric key inside your key vault and a symmetric key within SQL Server on your VM.</span></span> <span data-ttu-id="fb71a-104">Potom bude možné provést T-SQL příkazy pro zapnutí šifrování pro databáze a zálohování.</span><span class="sxs-lookup"><span data-stu-id="fb71a-104">Then, you will be able to execute T-SQL statements to enable encryption for your databases and backups.</span></span>

<span data-ttu-id="fb71a-105">Existuje několik typů šifrování, které můžete využít výhod:</span><span class="sxs-lookup"><span data-stu-id="fb71a-105">There are several forms of encryption you can take advantage of:</span></span>

* [<span data-ttu-id="fb71a-106">Transparentní šifrování dat (šifrování TDE)</span><span class="sxs-lookup"><span data-stu-id="fb71a-106">Transparent Data Encryption (TDE)</span></span>](https://msdn.microsoft.com/library/bb934049.aspx)
* [<span data-ttu-id="fb71a-107">Šifrované zálohování</span><span class="sxs-lookup"><span data-stu-id="fb71a-107">Encrypted backups</span></span>](https://msdn.microsoft.com/library/dn449489.aspx)
* [<span data-ttu-id="fb71a-108">Šifrování na úrovni sloupce (Vymazat)</span><span class="sxs-lookup"><span data-stu-id="fb71a-108">Column Level Encryption (CLE)</span></span>](https://msdn.microsoft.com/library/ms173744.aspx)

<span data-ttu-id="fb71a-109">Tyto skripty jazyka Transact-SQL zadejte příklady pro každé z těchto oblastí.</span><span class="sxs-lookup"><span data-stu-id="fb71a-109">The following Transact-SQL scripts provide examples for each of these areas.</span></span>

### <a name="prerequisites-for-examples"></a><span data-ttu-id="fb71a-110">Předpoklady pro příklady</span><span class="sxs-lookup"><span data-stu-id="fb71a-110">Prerequisites for examples</span></span>

<span data-ttu-id="fb71a-111">Každý příklad vychází z dva požadavky: asymetrického klíče z trezoru klíčů názvem **CONTOSO_KEY** pověření vytvořené funkci Integrace se službou AZURE s názvem **Azure_EKM_TDE_cred**.</span><span class="sxs-lookup"><span data-stu-id="fb71a-111">Each example is based on the two prerequisites: an asymmetric key from your key vault called **CONTOSO_KEY** and a credential created by the AKV Integration feature called **Azure_EKM_TDE_cred**.</span></span> <span data-ttu-id="fb71a-112">Tyto požadavky pro spuštění příkladů instalačního programu následující příkazy jazyka Transact-SQL.</span><span class="sxs-lookup"><span data-stu-id="fb71a-112">The following Transact-SQL commands setup these prerequisites for running the examples.</span></span>

``` sql
USE master;
GO

sp_configure [show advanced options], 1;
GO
RECONFIGURE;
GO

-- Enable EKM provider
sp_configure [EKM provider enabled], 1;
GO
RECONFIGURE;

--create provider
CREATE CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov
FROM FILE = 'C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll';
GO

--create credential
CREATE CREDENTIAL sysadmin_ekm_cred
    WITH IDENTITY = 'keytestvault', --keyvault
    SECRET = '<<SECRET>>'
FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov;


--must have sysadmin
ALTER LOGIN [TDE_Login]
ADD CREDENTIAL sysadmin_ekm_cred;


CREATE ASYMMETRIC KEY CONTOSO_KEY
FROM PROVIDER [AzureKeyVault_EKM_Prov]
WITH PROVIDER_KEY_NAME = 'keytestvault',  --key name
CREATION_DISPOSITION = OPEN_EXISTING;
```

### <a name="transparent-data-encryption-tde"></a><span data-ttu-id="fb71a-113">Transparentní šifrování dat (šifrování TDE)</span><span class="sxs-lookup"><span data-stu-id="fb71a-113">Transparent Data Encryption (TDE)</span></span>

1. <span data-ttu-id="fb71a-114">Vytvořit přihlášení systému SQL Server, který má být používána databázový stroj pro šifrování TDE a pak do ní přidejte přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="fb71a-114">Create a SQL Server login to be used by the Database Engine for TDE, then add the credential to it.</span></span>

   ``` sql
   USE master;
   -- Create a SQL Server login associated with the asymmetric key
   -- for the Database engine to use when it loads a database
   -- encrypted by TDE.
   CREATE LOGIN TDE_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the TDE Login to add the credential for use by the
   -- Database Engine to access the key vault
   ALTER LOGIN TDE_Login
   ADD CREDENTIAL Azure_EKM_TDE_cred;
   GO
   ```

1. <span data-ttu-id="fb71a-115">Vytvořte šifrovací klíč databáze, který se použije pro šifrování TDE.</span><span class="sxs-lookup"><span data-stu-id="fb71a-115">Create the database encryption key that will be used for TDE.</span></span>

   ``` sql
   USE ContosoDatabase;
   GO

   CREATE DATABASE ENCRYPTION KEY 
   WITH ALGORITHM = AES_128 
   ENCRYPTION BY SERVER ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the database to enable transparent data encryption.
   ALTER DATABASE ContosoDatabase
   SET ENCRYPTION ON;
   GO
   ```

### <a name="encrypted-backups"></a><span data-ttu-id="fb71a-116">Šifrované zálohování</span><span class="sxs-lookup"><span data-stu-id="fb71a-116">Encrypted backups</span></span>

1. <span data-ttu-id="fb71a-117">Vytvořit přihlášení systému SQL Server, který má být používána databázový stroj pro šifrování záloh a do něj přidat přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="fb71a-117">Create a SQL Server login to be used by the Database Engine for encrypting backups, and add the credential to it.</span></span>

   ``` sql
   USE master;
   -- Create a SQL Server login associated with the asymmetric key
   -- for the Database engine to use when it is encrypting the backup.
   CREATE LOGIN Backup_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the Encrypted Backup Login to add the credential for use by
   -- the Database Engine to access the key vault
   ALTER LOGIN Backup_Login
   ADD CREDENTIAL Azure_EKM_Backup_cred ;
   GO
   ```

1. <span data-ttu-id="fb71a-118">Zálohování databáze zadat šifrování s asymetrického klíče uložené v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="fb71a-118">Backup the database specifying encryption with the asymmetric key stored in the key vault.</span></span>

   ``` sql
   USE master;
   BACKUP DATABASE [DATABASE_TO_BACKUP]
   TO DISK = N'[PATH TO BACKUP FILE]'
   WITH FORMAT, INIT, SKIP, NOREWIND, NOUNLOAD,
   ENCRYPTION(ALGORITHM = AES_256, SERVER ASYMMETRIC KEY = [CONTOSO_KEY]);
   GO
   ```

### <a name="column-level-encryption-cle"></a><span data-ttu-id="fb71a-119">Šifrování na úrovni sloupce (Vymazat)</span><span class="sxs-lookup"><span data-stu-id="fb71a-119">Column Level Encryption (CLE)</span></span>

<span data-ttu-id="fb71a-120">Tento skript vytvoří symetrického klíče chráněné pomocí asymetrického klíče v trezoru klíčů a potom pomocí symetrický klíč k šifrování dat v databázi.</span><span class="sxs-lookup"><span data-stu-id="fb71a-120">This script creates a symmetric key protected by the asymmetric key in the key vault, and then uses the symmetric key to encrypt data in the database.</span></span>

``` sql
CREATE SYMMETRIC KEY DATA_ENCRYPTION_KEY
WITH ALGORITHM=AES_256
ENCRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;

DECLARE @DATA VARBINARY(MAX);

--Open the symmetric key for use in this session
OPEN SYMMETRIC KEY DATA_ENCRYPTION_KEY
DECRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;

--Encrypt syntax
SELECT @DATA = ENCRYPTBYKEY(KEY_GUID('DATA_ENCRYPTION_KEY'), CONVERT(VARBINARY,'Plain text data to encrypt'));

-- Decrypt syntax
SELECT CONVERT(VARCHAR, DECRYPTBYKEY(@DATA));

--Close the symmetric key
CLOSE SYMMETRIC KEY DATA_ENCRYPTION_KEY;
```

## <a name="additional-resources"></a><span data-ttu-id="fb71a-121">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="fb71a-121">Additional resources</span></span>

<span data-ttu-id="fb71a-122">Další informace o tom, jak používat tyto funkce šifrování najdete v tématu [pomocí EKM pomocí funkcí šifrování SQL serveru](https://msdn.microsoft.com/library/dn198405.aspx#UsesOfEKM).</span><span class="sxs-lookup"><span data-stu-id="fb71a-122">For more information on how to use these encryption features, see [Using EKM with SQL Server Encryption Features](https://msdn.microsoft.com/library/dn198405.aspx#UsesOfEKM).</span></span>

<span data-ttu-id="fb71a-123">Všimněte si, že postup v tomto článku předpokládá, že už máte SQL Server běžící na virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="fb71a-123">Note that the steps in this article assume that you already have SQL Server running on an Azure virtual machine.</span></span> <span data-ttu-id="fb71a-124">Pokud ne, najdete v části [zřízení virtuálního počítače s SQL serverem v Azure](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="fb71a-124">If not, see [Provision a SQL Server virtual machine in Azure](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md).</span></span> <span data-ttu-id="fb71a-125">Další pokyny k systémem SQL Server na virtuálních počítačích Azure, najdete v části [SQL Server na virtuálních počítačích Azure přehled](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fb71a-125">For other guidance on running SQL Server on Azure VMs, see [SQL Server on Azure Virtual Machines overview](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md).</span></span>