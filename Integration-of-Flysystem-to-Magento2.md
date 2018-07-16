# Adapters #
The Flysystem ist integrated to Magento2 using Adapters. This works with 2 different adapters.

* FilesystemManager which is to start a connection to the filesystem. Providing the functionality for a bunch of different Flysystem classes, because Flysystem uses one adapter for each source type.
* There the modularity should be provided, by dispatching an event before creating a connection with the Flysystem adapter, so you can integrate new adapters in your custom project
* FilesystemAdapter which is mostly a port for the Filesystem class of Flysystem and should provide all its methods. It provides the functionality to write, read, delete etc files from the source.
* FilesystemAdapterFactory which is used to generate a FilesystemAdapter this class should be used if you want to create an own module with the filesystem classes, or to provide a new Flysystem Adapter that is not integratet into this module.
* The Factory works with the return values of the FilesystemManager to return a new FilesystemAdapter.

```php
$this->_filesystemAdapter = $this->_filesystemAdapterFactory->create($this->_filesystemManager->createLocalDriver('/'));
```

# Blocks #
The Blocks Directory is devided into 2 subdirectorys

* Block/Filesystem provides the Block Classes for rendering the Flysystem Modal itself
* Block/Product is used to implement the modal into the product form, because thats not that easy to do with ui components. But more about this later.

# Controllers #
The Controllers inside Controller/Adminhtml/Filesystem provides all the functionality for the Flysystem Moal.

* Providing initial functionality like intantiating the Manager class: Index 
* Providing the content for the flysystem Modal: Contents, TreeJson
* Providing Flysystem Modal actions for buttons ans so on: DeleteFiles, DeleteFolder, NewFolder, OnInsert, Upload

# Helpers #
* Config provides all the configuration constants. An reads the adminpanel config with type parsing.
* Errors provides the error constants with parameters, so later an error guide could be provided, which makes debugging a lot more easy
* Filesystem provides all general methods which are used all over different classes inside the Flagbit Flysystem module, like decoding and encoding file hashes.

# Models #
The Model/Config/Source folders provides its only class Adapter which is used for rendering the select field in the adminpanel configuration for Flagbit Flysystem.

The Model/Filesystem directory provides the 3 like most important classes for the Flysystem modal.

* Manager is a singleton class which is used to read the configuration and provide the FilesystemAdapter for all the usage inside the Controllers,Blocks and Observers.
* It is used by DI and provides the method getAdapter, to get and/or create the FilesystemAdapter. It provides also access to the session to write some session variables.
* TmpManager is to create tmp files for the flysystem modal if you insert a file with the onInsert controller. It generates Flagbit Flysystem specific tmp files, but also transfers these tmp files to the product and category tmp folder.
* UploadManager is used to upload new files with the flysystem modal. It adapts the uploader class from magento, but the upload itself is also written using Flysystem, because the magento uploader dosn''t provide the functionality to upload files fe. to ans ftp server.