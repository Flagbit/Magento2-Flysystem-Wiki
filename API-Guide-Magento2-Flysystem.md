# Methods #

## FilesystemAdapter ##

see https://flysystem.thephpleague.com/docs/usage/filesystem-api/. While this is created to be an exact adapt of the Filesystem class of the Flysystem module. It may could be not 100% up to date at any time, so don''t be sure that every method works.

## FilesystemAdapterFactory ##

### create ###

* Parameters: \League\Flysystem\AdapterInterface $adapter, array $config
* Returns: \Flagbit\Flysystem\Adapter\FilesystemAdapter
* Description: The config array should be given to the FilesystemManager not the FilesystemManagerFactory. So in most cases you wo'nt need the config here.

## FilesystemManager ##

### createFtpDriver ###

* Parameters: array $config
* Returns: \League\Flysystem\Adapter\Ftp
* Description: for usage of the $config array visit: [https://flysystem.thephpleague.com/docs/adapter/ftp/](https://flysystem.thephpleague.com/docs/adapter/ftp/)

### createLocalDriver ###

* Parameters: string $root, int $writeFlags (default: LOCK_EX), int $linkHandling (default: \League\Flysystem\Adapter\Local::SKIP_LINKS), array $permissions
* Returns: \League\Flysystem\Adapter\Local
* Description: for usage of the parameters visit: [https://flysystem.thephpleague.com/docs/adapter/local/](https://flysystem.thephpleague.com/docs/adapter/local/)

### createNullDriver ###

* Parameters: none
* Returns: \League\Flysystem\Adapter\NullAdapter
* Description: Used for testing. For more information:[https://flysystem.thephpleague.com/docs/adapter/null-test/](https://flysystem.thephpleague.com/docs/adapter/null-test/)

# Events #

### flagbit_flysystem_create_after ###

Dispatched in

* Model/Filesystem/Manager create method

Variables

* source (string|null): The currently used source for the adapter. If the source parameter wasn't given to the modals create method, it will be the current configuration value.
* manager (\Flagbit\Flysystem\Model\Filesystem\Manager): The object of the Manager class which dispatched the event.

Used for

* Integrate own Flysystem Adapters into the current module. It is never used in the module itself

### flagbit_flysystem_oninsert_after ###

Dispatched in

* Controller/Adminhtml/Filesystem/OnInsert execute method

Variables

* **controller (\Flagbit\Flysystem\Controller\Adminhtml\Filesystem\OnInsert):** The object of the OnInsert controller which dispatched the event
* **filename (string):** The current filename to insert (with its path, decoded)
* **manager (\Flagbit\Flysystem\Model\Filesystem\Manager):** The current manager singleton which provides the FilesystemAdapter
* **modal_id (string):** The modal identifier set inside the modal, used to identify which modal dispatched the event

Used for

* Transfering the uploaded file to the correct TmpFolder. Inside the Flysystem module it is used for the CategoryImage und ProductImage Observers to transfer the uploaded file to the category or product media tmp folder