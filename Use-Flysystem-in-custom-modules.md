To use Flysystem in your custom module you need the Adapter classes in Flagbit/Flysystem/Adapter.
There you got these 3 classes.

* FilesystemAdapter
* FilesystemAdapterFactory
* FilesystemManager

To create an own Adapter in your custom Module you need to require the FilesystemAdapterFactory and FilesystemManager.

With the FilesystemManager you can create your driver. F.e a localDriver.
The FilesystemManager is the Adaption for the Flysystem Local-,Ftp- and Null Adapter classes. If you have already created an own FilesystemManager for another Adapter you want to use in your custom module, like described [here](https://bitbucket.org/flagbit/magento2-flysystem/wiki/Integrate%20a%20new%20Flysystem%20Adapter), use this one.

After you created an Driver, you are still not able to use it. You need to pass the Driver to the FilesystemAdapterFactory to create an FilesystemAdapter. The FilesystemAdapterFactory and FilesystemAdapter are the adaption of the League/Flysystem/Filesystem class of Flysystem.
It can be used like the modules class [itself](https://flysystem.thephpleague.com/docs/usage/filesystem-api/).

Visit the other guides for more information.

*Here is an Code example how to create your own Flysystem Adapter:*

```php
<?php
namespace MyCompany\MyModule\Model;
 
 
class Flysystem
{
    protected $_flysystemManager;
    protected $_flysystemAdapterFactory;

    protected $_adapter;
 
    public function __construct(
        \Flagbit\Flysystem\Adapter\FilesystemManager $flysystemManager,
        \Flagbit\Flysystem\Adapter\FilesystemAdapterFactory $flysystemAdapterFactory
    ) {
        $this->_flysystemManager = $sftpManager;
        $this->_flysystemAdapterFactory = $flysystemFactory;
    }
 
    /**
     * Create a new Flysystem FilesystemAdapter and return it
     */
    public function getFlysystemAdapter()
    {
        if(!$this->_adapter) {
            try {
                $localRoot = '/var/www/magentoshop/htdocs/pub/media';

                $this->_adapter = $this->_flysystemAdapterFactory->create($this->_flysystemManager->createLocalDriver($localRoot));
            } catch (\Exception $e) {
                throw new \Exception('Something went wrong while creating a new adapter.');
            }
        }
        
        return $adapter;
    }

    /**
     * This will read the file from /var/www/magentoshop/htdocs/pub/media/mymodule/config.xml
     */
    public function readConfigfile()
    {
        $filePath = '/mymodule/config.xml';
        
        if(!$this->_adapter) {
            throw new \Exception('Adapter is not initialized!');
        }

        try {
            $contents = $this->_adapter->read($filePath);
        } catch (\Exception $e) {
            throw new \Exception('Something went wrong while reading config file.');
        }
        return $contents;
    }
}
```