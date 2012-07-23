Description
-----------

_Baobaz EMS_ synchronizes Magento with Experian CheetahMail.

Note: EMS = Emailing Solution.

Features list:

* Synchronize Magento Newsletter subscribers (anonymous or customers) with CheetahMail database through the Soap WebService
* Fields mapping between Magento customers' attributes and CheetahMail fields
* CheetahMail fields list (+ description) autoloaded in backend
* Opt'in dedicated field


Configuration
-------------

* Config
 * System > Configuration > Customers > Newsletter > EMS Settings
  * Login: EMS Soap account login
  * Password: EMS Soap account password
  * List ID: EMS database ID
  * Use proxy: if enabled, add proxy IP and port
  * Field Mapping: mapping between Magento Customers' attributes and CheetahMail fields
* Config file (config.xml)
 * global > settings > ems > soap
  * wsdl


Screenshot
----------

![Baobaz_Ems Configuration](https://github.com/Baobaz/Magento_Baobaz_Ems/raw/master/doc/screenshots/Baobaz_Ems-Configuration_4.png "Baobaz_Ems Configuration")


Rewrite
-------

app\code\local\{Namespace}\Ems\etc\config.xml:
``` xml
<global>
    <models>
        <baobaz_ems>
            <rewrite>
                <adapter_customer>{Namespace}_Ems_Model_Adapter_Customer</adapter_customer>
            </rewrite>
        </baobaz_ems>
    </models>
</global>
```

app\code\local\{Namespace}\Ems\Model\Adapter\Customer.php:
``` php
<?php
class {Namespace}_Ems_Model_Adapter_Customer extends Baobaz_Ems_Model_Adapter_Customer
{
    /**
     * Get specific attributes
     */
    public function _getAttributes()
    {
        $attributes = parent::_getAttributes();
        $attributes['attribute_1_code'] = 'attribute_1_name';
        $attributes['attribute_2_code'] = 'attribute_2_name';
        return $attributes;
    }
    /**
     * Civility field specific mapping
     */
    public function getPrefix()
    {
        $prefix = $this->getCustomer()->getPrefix();
        switch ($prefix) {
            case 'MR':
                $value = 'Monsieur';
                break;
            case 'MME':
                $value = 'Madame';
                break;
            case 'MLE':
                $value = 'Mademoiselle';
                break;
            default:
                $value = '';
        }
        return $value;
    }
}
```