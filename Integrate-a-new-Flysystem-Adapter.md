* Check http://flysystem.thephpleague.com/docs/ if there is an Adapter available for the Filesystem you want to use
* Add the require statement defined in the Adapter Documentation of Flysystem into your Magento2 composer.json and run composer update
* Create a new custom module f.e. MyCompany/FlysystemSFTP and the needed files for a new module
* Create a new FilesystemManager that extends Flagbit\Flysystem\Adapter\FilesystemManager and implement a create method for your new Adapter 

```php
<?php
namespace MyCompany\FlysystemSFTP\Adapter;
 
 
class SFTPManager extends \Flagbit\Flysystem\Adapter\FilesystemManager
{
    public function createSftpDriver(array $config)
    {
        // do the creation stuff here for the sftp driver
    }
}
```

* Add a etc/adminhtml/system.xml file inside your custom module to extend the configuration for all the new fields you will need
* Add a etc/adminhtml/di.xml file to extend the di.xml type for Flagbit\Flysystem\Model\Config\Source\Adapter. This is to create a new entry for the Flysystem Source Configuration
* Add a Observer to hook the event flagbit_flysystem_create_after and define it in your custom modules etc/adminhtml/events.xml

```php
<?php
namespace MyCompany\FlysystemSFTP\Observer;
 
 
class SFTPObserver extends \Magento\Framework\Event\ObserverInterface
{
    protected $_sftpManager;
 
 
    protected $_flysystemFactory;
 
 
    public function __construct(
        \MyCompany\FlysystemSFTP\Adapter\SFTPManager $sftpManager,
        \Flagbit\Flysystem\Adapter\FilesystemAdapterFactory $flysystemFactory
    ) {
        $this->_sftpManager = $sftpManager;
        $this->_flysystemFactory = $flysystemFactory;
    }
 
 
    public function execute(\Magento\Framework\Event\Observer $observer)
    {
        $manager = $observer->getEvent()->getData('manager');
        $source = $observer->getEvent()->getData('source');
 
 
        if($source === 'sftp')
        {
            $config = (...); // get and parse the store config here and don't forget to do some security stuff
            $adapter = $this->_flysystemFactory->create($this->_sftpManager->createSftpDriver($config));
            $manager->setAdapter($adapter);
        }
    }
}
```

* That should be all. Configure it and test it.