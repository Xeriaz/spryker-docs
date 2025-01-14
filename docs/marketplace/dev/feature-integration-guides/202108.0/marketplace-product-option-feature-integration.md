---
title: Marketplace Product Option feature integration
last_updated: Jul 28, 2021
Description: This document describes the process how to integrate the Marketplace Product Option feature into a Spryker project.
template: feature-integration-guide-template
---

This document describes how to integrate the Marketplace Product Option feature into a Spryker project.


## Install feature core

Follow the steps below to install the Marketplace Product Option feature core.

### Prerequisites

To start feature integration, integrate the required features:

| NAME | VERSION | INTEGRATION GUIDE |
| --------------- | ------- | ---------- |
| Spryker Core         | {{page.version}} | [Spryker Core feature integration](/docs/scos/dev/feature-integration-guides/{{page.version}}/spryker-core-feature-integration.html) |
| Product Options      | {{page.version}} | [Product Options feature integration](https://spryker.atlassian.net/wiki/spaces/DOCS/pages/903151851/EMPTY+Product+Options+Feature+Integration+-+ongoing) |  
| Marketplace Merchant | {{page.version}} | [Marketplace Merchant feature integration](/docs/marketplace/dev/feature-integration-guides/{{page.version}}/marketplace-merchant-feature-integration.html) |


### 1) Install the required modules using Composer

1) Install the required modules:

```bash
composer require spryker-feature/marketplace-product-options:"{{page.version}}" --update-with-dependencies
```

{% info_block warningBox "Verification" %}

Make sure the following modules have been installed:

| MODULE | EXPECTED DIRECTORY |
|-|-|
| MerchantProductOption | vendor/spryker/merchant-product-option |
| MerchantProductOptionDataImport | vendor/spryker/merchant-product-option-data-import |
| MerchantProductOptionGui | vendor/spryker/merchant-product-option-gui |
| MerchantProductOptionStorage | vendor/spryker/merchant-product-option-storage |

{% endinfo_block %}


### 2) Set up the database schema and transfer objects

Adjust the schema definition to guarantee unique identifier for each option group:

**src/Pyz/Zed/DataImport/Persistence/Propel/Schema/spy_product_option.schema.xml**

```xml
<?xml version="1.0"?>
<database xmlns="spryker:schema-01" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" name="zed" xsi:schemaLocation="spryker:schema-01 https://static.spryker.com/schema-01.xsd" namespace="Orm\Zed\ProductOption\Persistence" package="src.Orm.Zed.ProductOption.Persistence">

    <table name="spy_product_option_group" identifierQuoting="true">
        <column name="key" type="VARCHAR" size="255" description="Key is an identifier for existing entities. This should never be changed."/>

        <unique name="spy_product_option_group-unique-key">
            <unique-column name="key"/>
        </unique>
    </table>
</database>

```

Apply database changes and to generate entity and transfer changes:

```bash
console propel:install
console transfer:generate
```


{% info_block warningBox "Verification" %}

Verify that the following changes have been implemented by checking your database:

| DATABASE ENTITY | TYPE | EVENT |
|-|-|-|
| spy_merchant_product_option_group | table | created |
| spy_product_option_group.key | column | created |

Make sure that the following changes were applied in transfer objects:

| TRANSFER  | TYPE  | EVENT | PATH |
| - | - | - | - |
| MerchantProductOptionGroup | class | created | src/Generated/Shared/Transfer/MerchantProductOptionGroupTransfer |
| MerchantProductOptionGroupCriteria | class | created | src/Generated/Shared/Transfer/MerchantProductOptionGroupCriteriaTransfer |
| MerchantProductOptionGroupCollection | class | created | src/Generated/Shared/Transfer/MerchantProductOptionGroupCollectionTransfer |
| ProductOptionGroup.merchant | attribute | created | src/Generated/Shared/Transfer/ProductOptionGroupTransfer |

{% endinfo_block %}

### 3) Add translations

Append glossary according to your configuration:

**data/import/common/common/glossary.csv**

```yaml
checkout.item.option.pre.condition.validation.error.exists,"Product option of %name% is not available anymore.",en_US
checkout.item.option.pre.condition.validation.error.exists,"Produktoption von %name% ist nicht mehr verfügbar.",de_DE
```

Import data:

```bash
console data:import glossary
```

{% info_block warningBox "Verification" %}

Make sure that the configured data is added to the `spy_glossary_key` and `spy_glossary_translation` tables in the database.

{% endinfo_block %}

### 4) Import data

Prepare your data according to your requirements using the demo data:

**data/import/common/common/marketplace/merchant_product_option_group.csv**

```
product_option_group_key,merchant_reference,approval_status,merchant_sku
insurance,MER000001,approved,spr-425453
```

Add the `product_option_group_key` column to product_option.csv:

**data/import/common/common/product_option.csv**

```
abstract_product_skus,option_group_id,tax_set_name,group_name_translation_key,group_name.en_US,group_name.de_DE,option_name_translation_key,option_name.en_US,option_name.de_DE,sku,product_option_group_key
"M74972,M74974,M1000786,M1000838,M1000839,M1000840,M302,M75601,M11222,M39654,M1896,M62305,M80392,M857,M856,M858,M859,M54107,M54110,M54111,M54113,M55895,M55936,M55937,M58227,M58228,M58230,M58312,M58313,M58315,M58314,M58335,M58336,M68854,M62301,M62306,M68726,M68727,M852,M75685,M75686,M75727,M75700,M75730,M40490,M40491,M213,M226,M229,M14054,M15127,M87464,M87416,M731,M12655,M58035,M58036,M58037,M58038,M62584,M634,M635,M636,M74966,M90806,M10569,M10577,M11473,M11306,M75071,M11480,M526,M528,M45050,M49207,M49344,M58100,M58102,M61124,M61125,M66923,M64947,M69408,M58095,M72878,M74907,M53331,M10871,M10872,M2365,M2366,M2393,M64933,M64934,M86938,M86939,M1000785,M53238,M74815,M719,M717,M714,M715,M716,M718,M766,M767,M768,M769,M770,M771,M787,M788,M789,M53288,M53287,M2495,M61240,M79890,M79892,M79932,M72933,M72935,M72937,M72938,M72939,M72940,M72942,M72943,M72944,M72945,M5022,M5043,M5044,M70208,M5154,M5155,M5108,M5113,M5148,M5147,M1006484,M65062,M88181,M88182,M72843,M72844,M1006871,M43347,M90737,M90802,M90799,M90800,M90804,M90805,M1006811,M78380,M49320,M90850,M90851,M1014646,M1014645,M1014629,M1014642,M1014643,M1014644,M1014653,M1015355,M1015356,M1014656,M88184,M1014948,M90810,M432,M87599,M1014496,M1014502,M1002203,M39658,M15226,M1014866,M1014871,M1014870,M1014874,M1017945,M1018212,M1018211,M1017902,M1017905,M1022640,M1022643,M1022632,M1024601,M1024684,M1024689,M1028483,M1028095,M1028702,M1030133,M1028049,M1028061,M1028062,M1028038,M1031236,M1031238,M1028375,M1028376,M21062,M21066,M21067,M21073,M21090,M21091,M21092,M21093,M21094,M21097,M21099,M21100,M21101,M21103,M21104,M21105,M21189,M21190,M21191,M21193,M21194,M21195,M21196,M21197,M21200,M21201,M21202,M21203,M21205,M21296,M21299,M21635,M21636,M21637,M21638,M21646,M21648,M21650,M21679,M21680,M21681,M21684,M21692,M21693,M21694,M21703,M21704,M21705,M21708,M21711,M21713,M21714,M21715,M21716,M21717,M21740,M21741,M21742,M21743,M21744,M21745,M21759,M21766,M21771,M21772,M21773,M21774,M21777,M21778,M21779,M21903,M22230,M22232,M22269,M22293,M22397,M22398,M22410,M22411,M22412,M22432,M22484,M22613,M22663,M22664,M22682,M22796,M22797,M23067,M23325,M23330,M23340,M23343,M23345,M23432,M23436,M23438,M23440,M23441,M23444,M23450,M23451,M23454,M23455,M23456,M23457,M23459,M23460,M23461,M23462,M23463,M23467,M23468,M23469,M23470,M23471,M23472,M23473,M23478,M23479,M23482,M23484,M23486,M23487,M23488,M23510,M23512,M23513,M23720,M23721,M23722,M23723,M23724,M23725,M23726,M23727,M23729,M23730,M23731,M23732,M23751,M23764,M23766,M23770,M23771,M23777,M24143,M24416,M24417,M24418,M24563,M24605,M24606,M24609,M24610,M24612,M24613,M24632,M24637,M25598,M25599,M25600,M1013168,M1013287,M1013541,M1016129,M26415,M26416,M26417,M26418,M26419,M26420,M1079219,M1079221,M1079222,M1079243,M1080190,M87,M26831,M29455,M29456,M29468,M29499,M29501,M29502,M29503,M29504,M29507,M29523,M29524,M29525,M29526,M29529,M29530,M29929,M29930,M29931,M53235,M11542,M111,C2235,C2237",1,Entertainment Electronics,product.option.group.name.warranty,Warranty,Garantie,product.option.warranty_1,One (1) year limited warranty,Ein (1) Jahr begrenzte Garantie,OP_1_year_waranty,warranty
,1,Entertainment Electronics,product.option.group.name.warranty,Warranty,Garantie,product.option.warranty_2,Two (2) year limited warranty,Zwei (2) Jahre begrenzte Garantie,OP_2_year_waranty,warranty
,1,Entertainment Electronics,product.option.group.name.warranty,Warranty,Garantie,product.option.warranty_3,Three (3) year limited warranty,Drei (3) Jahre begrenzte Garantie,OP_3_year_waranty,warranty
"M74972,M74974,M1000786,M1000838,M1000839,M1000840,M302,M75601,M11222,M39654,M1896,M62305,M80392,M857,M856,M858,M859,M54107,M54110,M54111,M54113,M55895,M55936,M55937,M58227,M58228,M58230,M58312,M58313,M58315,M58314,M58335,M58336,M68854,M62301,M62306,M68726,M68727,M852,M75685,M75686,M75727,M75700,M75730,M40490,M40491,M213,M226,M229,M14054,M15127,M87464,M87416,M731,M12655,M58035,M58036,M58037,M58038,M62584,M634,M635,M636,M74966,M90806,M10569,M10577,M11473,M11306,M75071,M11480,M526,M528,M45050,M49207,M49344,M58100,M58102,M61124,M61125,M66923,M64947,M69408,M58095,M72878,M74907,M53331,M10871,M10872,M2365,M2366,M2393,M64933,M64934,M86938,M86939,M1000785,M53238,M74815,M719,M717,M714,M715,M716,M718,M766,M767,M768,M769,M770,M771,M787,M788,M789,M53288,M53287,M2495,M61240,M79890,M79892,M79932,M72933,M72935,M72937,M72938,M72939,M72940,M72942,M72943,M72944,M72945,M5022,M5043,M5044,M70208,M5154,M5155,M5108,M5113,M5148,M5147,M1006484,M65062,M88181,M88182,M72843,M72844,M1006871,M43347,M90737,M90802,M90799,M90800,M90804,M90805,M1006811,M78380,M49320,M90850,M90851,M1014646,M1014645,M1014629,M1014642,M1014643,M1014644,M1014653,M1015355,M1015356,M1014656,M88184,M1014948,M90810,M432,M87599,M1014496,M1014502,M1002203,M39658,M15226,M1014866,M1014871,M1014870,M1014874,M1017945,M1018212,M1018211,M1017902,M1017905,M1022640,M1022643,M1022632,M1024601,M1024684,M1024689,M1028483,M1028095,M1028702,M1030133,M1028049,M1028061,M1028062,M1028038,M1031236,M1031238,M1028375,M1028376,M21062,M21066,M21067,M21073,M21090,M21091,M21092,M21093,M21094,M21097,M21099,M21100,M21101,M21103,M21104,M21105,M21189,M21190,M21191,M21193,M21194,M21195,M21196,M21197,M21200,M21201,M21202,M21203,M21205,M21296,M21299,M21635,M21636,M21637,M21638,M21646,M21648,M21650,M21679,M21680,M21681,M21684,M21692,M21693,M21694,M21703,M21704,M21705,M21708,M21711,M21713,M21714,M21715,M21716,M21717,M21740,M21741,M21742,M21743,M21744,M21745,M21759,M21766,M21771,M21772,M21773,M21774,M21777,M21778,M21779,M21903,M22230,M22232,M22269,M22293,M22397,M22398,M22410,M22411,M22412,M22432,M22484,M22613,M22663,M22664,M22682,M22796,M22797,M23067,M23325,M23330,M23340,M23343,M23345,M23432,M23436,M23438,M23440,M23441,M23444,M23450,M23451,M23454,M23455,M23456,M23457,M23459,M23460,M23461,M23462,M23463,M23467,M23468,M23469,M23470,M23471,M23472,M23473,M23478,M23479,M23482,M23484,M23486,M23487,M23488,M23510,M23512,M23513,M23720,M23721,M23722,M23723,M23724,M23725,M23726,M23727,M23729,M23730,M23731,M23732,M23751,M23764,M23766,M23770,M23771,M23777,M24143,M24416,M24417,M24418,M24563,M24605,M24606,M24609,M24610,M24612,M24613,M24632,M24637,M25598,M25599,M25600,M1013168,M1013287,M1013541,M1016129,M26415,M26416,M26417,M26418,M26419,M26420,M1079219,M1079221,M1079222,M1079243,M1080190,M87,M26831,M29455,M29456,M29468,M29499,M29501,M29502,M29503,M29504,M29507,M29523,M29524,M29525,M29526,M29529,M29530,M29929,M29930,M29931,M53235,M11542,M111,C2235,C2237,B0001,B0002",2,Entertainment Electronics,product.option.group.name.insurance,Insurance,Versicherungsschutz,product.option.insurance,Two (2) year insurance coverage,Zwei (2) Jahre Versicherungsschutz,OP_insurance,insurance
```

#### Register data importer:

**data/import/local/full_EU.yml**

```yml
version: 0

actions:
    - data_entity: merchant-product-option-group
      source: data/import/common/common/marketplace/merchant_product_option_group.csv
```


**data/import/local/full_US.yml**

```yml
version: 0

actions:
    - data_entity: merchant-product-option-group
      source: data/import/common/common/marketplace/merchant_product_option_group.csv
```


Register the following plugin to enable the data import:

| PLUGIN | DESCRIPTION | PREREQUISITES | NAMESPACE |
|-|-|-|-|
| MerchantProductOptionGroupDataImportPlugin | Validates Merchant reference and inserts merchant product option groups into the datanbase. | None | Spryker\Zed\MerchantProductOptionDataImport\Communication\Plugin\DataImport |

**src/Pyz/Zed/DataImport/DataImportDependencyProvider.php**

```php
<?php

namespace Pyz\Zed\DataImport;

use Spryker\Zed\MerchantProductOptionDataImport\Communication\Plugin\DataImport\MerchantProductOptionGroupDataImportPlugin;
use Spryker\Zed\DataImport\DataImportDependencyProvider as SprykerDataImportDependencyProvider;

class DataImportDependencyProvider extends SprykerDataImportDependencyProvider
{
    /**
     * @return array
     */
    protected function getDataImporterPlugins(): array
    {
        return [
            new MerchantProductOptionGroupDataImportPlugin(),
        ];
    }
}
```

Modify `ProductOptionWriterStep`:

**src/Pyz/Zed/DataImport/Business/Model/ProductOption/ProductOptionWriterStep.php**

```php
<?php

namespace Pyz\Zed\DataImport\Business\Model\ProductOption;

use Pyz\Zed\DataImport\Business\Model\Tax\TaxSetNameToIdTaxSetStep;
use Spryker\Zed\DataImport\Business\Model\DataImportStep\DataImportStepInterface;
use Spryker\Zed\DataImport\Business\Model\DataImportStep\PublishAwareStep;

class ProductOptionWriterStep extends PublishAwareStep implements DataImportStepInterface
{
    ...
    public function execute(DataSetInterface $dataSet)
    {
        $productOptionGroupEntity = SpyProductOptionGroupQuery::create()
            ->filterByName($dataSet[self::KEY_GROUP_NAME_TRANSLATION_KEY])
            ->filterByKey($dataSet[self::KEY_PRODUCT_OPTION_GROUP_KEY])
            ->findOneOrCreate();

        $productOptionGroupEntity
            ->setName($dataSet[static::KEY_OPTION_NAME_TRANSLATION_KEY])
            ->setActive($this->isActive($dataSet, $productOptionGroupEntity))
            ->setFkTaxSet($dataSet[TaxSetNameToIdTaxSetStep::KEY_TARGET])
            ->save();
        ...
    }
}
```

Import data:

```bash
console data:import merchant-product-option-group
```

{% info_block warningBox "Verification" %}

Make sure that the Merchant Product Option Group data is in the `spy_merchant_product_option_group` table.

{% endinfo_block %}

### 5) Set up behavior

Enable the following behaviors by registering the plugins:

| PLUGIN | DESCRIPTION | PREREQUISITES | NAMESPACE |
|-|-|-|-|
| MerchantProductOptionListActionViewDataExpanderPlugin | Expands data with the merchant collection. | None | Spryker\Zed\MerchantGui\Communication\Plugin\ProductOptionGui |
| MerchantProductOptionListTableQueryCriteriaExpanderPlugin | Extends `QueryCriteriaTransfer` with the merchant product option group criteria for expanding default query running in `ProductOptionListTable`. | None | Spryker\Zed\MerchantProductOptionGui\Communication\Plugin\ProductOptionGui |
| MerchantProductOptionGroupExpanderPlugin | Expands a product option group data with the related merchant. | None | Spryker\Zed\MerchantProductOption\Communication\Plugin\ProductOption |
| MerchantProductOptionCollectionFilterPlugin | Filters merchant product option group transfers by approval status and excludes the product options with the not approved merchant groups. | None | Spryker\Zed\MerchantProductOptionStorage\Communication\Plugin\ProductOptionStorage |
| MerchantProductOptionGroupWritePublisherPlugin | Retrieves all abstract product IDs using the  merchant product option group IDs from the event transfers. | None | Spryker\Zed\MerchantProductOptionStorage\Communication\Plugin\Publisher\MerchantProductOption |


<details>
<summary markdown='span'>src/Pyz/Zed/ProductOption/ProductOptionDependencyProvider.php</summary>

```php
<?php

namespace Pyz\Zed\ProductOption;

use Spryker\Zed\MerchantGui\Communication\Plugin\ProductOptionGui\MerchantProductOptionListActionViewDataExpanderPlugin;
use Spryker\Zed\MerchantProductOption\Communication\Plugin\ProductOption\MerchantProductOptionGroupExpanderPlugin;
use Spryker\Zed\MerchantProductOptionGui\Communication\Plugin\ProductOptionGui\MerchantProductOptionListTableQueryCriteriaExpanderPlugin;
use Spryker\Zed\ProductOption\ProductOptionDependencyProvider as SprykerProductOptionDependencyProvider;

class ProductOptionDependencyProvider extends SprykerProductOptionDependencyProvider
{
    /**
     * @return array<\Spryker\Zed\ProductOptionGuiExtension\Dependency\Plugin\ProductOptionListActionViewDataExpanderPluginInterface>
     */
    protected function getProductOptionListActionViewDataExpanderPlugins(): array
    {
        return [
            new MerchantProductOptionListActionViewDataExpanderPlugin(),
        ];
    }

    /**
     * @return array<\Spryker\Zed\ProductOptionGuiExtension\Dependency\Plugin\ProductOptionListTableQueryCriteriaExpanderPluginInterface>
     */
    protected function getProductOptionListTableQueryCriteriaExpanderPlugins(): array
    {
        return [
            new MerchantProductOptionListTableQueryCriteriaExpanderPlugin(),
        ];
    }

    /**
     * @return array<\Spryker\Zed\ProductOptionExtension\Dependency\Plugin\ProductOptionGroupExpanderPluginInterface>
     */
    protected function getProductOptionGroupExpanderPlugins(): array
    {
        return [
            new MerchantProductOptionGroupExpanderPlugin(),
        ];
    }
}
```

</details>

**src/Pyz/Zed/ProductOptionStorage/ProductOptionStorageDependencyProvider.php**

```php
<?php

namespace Pyz\Zed\ProductOptionStorage;

use Spryker\Zed\MerchantProductOptionStorage\Communication\Plugin\ProductOptionStorage\MerchantProductOptionCollectionFilterPlugin;
use Spryker\Zed\ProductOptionStorage\ProductOptionStorageDependencyProvider as SprykerProductOptionStorageDependencyProvider;

class ProductOptionStorageDependencyProvider extends SprykerProductOptionStorageDependencyProvider
{
    /**
     * @return array<\Spryker\Zed\ProductOptionStorageExtension\Dependency\Plugin\ProductOptionCollectionFilterPluginInterface>
     */
    protected function getProductOptionCollectionFilterPlugins(): array
    {
        return [
            new MerchantProductOptionCollectionFilterPlugin(),
        ];
    }
}
```

**src/Pyz/Zed/Publisher/PublisherDependencyProvider.php**

```php
<?php

namespace Pyz\Zed\Publisher;

use Spryker\Zed\MerchantProductOptionStorage\Communication\Plugin\Publisher\MerchantProductOption\MerchantProductOptionGroupWritePublisherPlugin;
use Spryker\Zed\Publisher\PublisherDependencyProvider as SprykerPublisherDependencyProvider;

class PublisherDependencyProvider extends SprykerPublisherDependencyProvider
{
    /**
     * @return array
     */
    protected function getPublisherPlugins(): array
    {
        return [
            $this->getMerchantProductOptionStoragePlugins(),
        ];
    }

    /**
     * @return array<\Spryker\Zed\PublisherExtension\Dependency\Plugin\PublisherPluginInterface>
     */
    protected function getMerchantProductOptionStoragePlugins(): array
    {
        return [
            new MerchantProductOptionGroupWritePublisherPlugin(),
        ];
    }
}
```

{% info_block warningBox "Verification" %}

Make sure merchants can create product option groups and values in the Merchant Portal.
Make sure that merchant product option information is shown on product details page when it is approved and active.
Make sure that merchant product option information is displayed in the cart, checkout, and user account.
Make sure that merchant product options are a part of the marketplace/merchant order and all totals are calculated correctly.

{% endinfo_block %}

## Related features

| FEATURE | REQUIRED FOR THE CURRENT FEATURE | INTEGRATION GUIDE |
| -------------- | -------------------------------- | ----------------- |
| Marketplace Product Option + Cart | | [Marketplace Product Option + Cart feature integration](/docs/marketplace/dev/feature-integration-guides/{{page.version}}/marketplace-product-option-cart-feature-integration.html) |
| Marketplace Product Option + Checkout | | [Marketplace Product Option + Checkout feature integration](/docs/marketplace/dev/feature-integration-guides/{{page.version}}/marketplace-product-option-checkout-feature-integration.html) |
