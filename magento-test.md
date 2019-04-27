# Test

## Create command line module which will create configurable product:

- configurable product should have 3 custom attributes
- configurable product should include 2 different simple products.


### Test 1: Create command line module which will create configurable product.

```
$objectManager = \Magento\Framework\App\ObjectManager::getInstance(); // instance of object manager
$product = $objectManager->create('\Magento\Catalog\Model\Product');
$product1 = $objectManager->create('\Magento\Catalog\Model\Product');
$product->setSku('my-sku'); // Set your sku here
$product->setName('Sample Simple Product'); // Name of Product
$product->setAttributeSetId(1); // Attribute set id
$product->setStatus(1); // Status on product enabled/ disabled 1/0
$product->setWeight(10); // weight of product
$product->setVisibility(4); // visibilty of product (catalog / search / catalog, search / Not visible individually)
$product->setTaxClassId(0); // Tax class id
$product->setTypeId('simple'); // type of product (simple/virtual/downloadable/configurable)
$product->setPrice(100); // price of product
$product->setStockData(
                        array(
                            'use_config_manage_stock' => 0,
                            'manage_stock' => 1,
                            'is_in_stock' => 1,
                            'qty' => 999999999
                        )
                    );
// Adding Image to product
$imagePath = "product.jpg"; // path of the image
$product->addImageToMediaGallery($imagePath, array('image', 'small_image', 'thumbnail'), false, false);

$product1->setSku('my-sku'); 
$product1->setName('Sample Simple Product'); 
$product1->setAttributeSetId(5);
$product1->setStatus(1); 
$product1->setWeight(10);
$product1->setVisibility(4);
$product1->setTaxClassId(0);
$product1->setTypeId('simple');
$product1->setPrice(100);
$product1->setStockData(
                        array(
                            'use_config_manage_stock' => 0,
                            'manage_stock' => 1,
                            'is_in_stock' => 1,
                            'qty' => 888888888
                        )
                    );
$imagePath1 = "product1.jpg"; // path of the image
$product1->addImageToMediaGallery($imagePath1, array('image', 'small_image', 'thumbnail'), false, false);

$product->save();
$product1->save();



// Adding Custom option to product
$options = array(
                array(
                    "sort_order"    => 1,
                    "title"         => "Option 1",
                    "price_type"    => "fixed",
                    "price"         => "150",
                    "type"          => "field",
                    "is_require"   => 0
                ),
                array(
                    "sort_order"    => 2,
                    "title"         => "Opion 2",
                    "price_type"    => "fixed",
                    "price"         => "200",
                    "type"          => "field",
                    "is_require"   => 0
                )
               array(
                    "sort_order"    => 3,
                    "title"         => "Opion 3",
                    "price_type"    => "fixed",
                    "price"         => "200",
                    "type"          => "field",
                    "is_require"   => 0
                )
            );
foreach ($options as $arrayOption) {
    $product->setHasOptions(1);
    $product->getResource()->save($product);
    $option = $objectManager->create('\Magento\Catalog\Model\Product\Option')
                    ->setProductId($product->getId())
                    ->setStoreId($product->getStoreId())
                    ->addData($arrayOption);
    $option->save();
    $product->addOption($option);
}


// Creating configurable product
$productId = 1; // Configurable Product Id
$objectManager = \Magento\Framework\App\ObjectManager::getInstance();
$product = $objectManager->create('Magento\Catalog\Model\Product')->load($productId); // Load Configurable Product
$attributeModel = $objectManager->create('Magento\ConfigurableProduct\Model\Product\Type\Configurable\Attribute');
$position = 0;
$attributes = array(134, 135); // Super Attribute Ids Used To Create Configurable Product
$associatedProductIds = array(2,4,5,6); //Product Ids Of Associated Products
foreach ($attributes as $attributeId) {
    $data = array('attribute_id' => $attributeId, 'product_id' => $productId, 'position' => $position);
    $position++;
    $attributeModel->setData($data)->save();
}
$product->setTypeId("configurable"); // Setting Product Type As Configurable
$product->setAffectConfigurableProductAttributes(4);
$objectManager->create('Magento\ConfigurableProduct\Model\Product\Type\Configurable')->setUsedProductAttributeIds($attributes, $product);
$product->setNewVariationsAttributeSetId(4); // Setting Attribute Set Id
$product->setAssociatedProductIds($associatedProductIds);// Setting Associated Products
$product->setCanSaveConfigurableAttributes(true);
$product->save();


$productId1 = 2; // Configurable Product Id
$objectManager1 = \Magento\Framework\App\ObjectManager::getInstance();
$product1 = $objectManager->create('Magento\Catalog\Model\Product')->load($productId1); // Load Configurable Product
$attributeModel1 = $objectManager->create('Magento\ConfigurableProduct\Model\Product\Type\Configurable\Attribute');
$position1 = 0;
$attributes1 = array(134, 135); // Super Attribute Ids Used To Create Configurable Product
$associatedProductIds1 = array(2,4,5,6); //Product Ids Of Associated Products
foreach ($attributes1 as $attributeId) {
    $data = array('attribute_id' => $attributeId, 'product_id' => $productId, 'position' => $position1);
    $position1++;
    $attributeModel1->setData($data)->save();
}
$product1->setTypeId("configurable"); // Setting Product Type As Configurable
$product1->setAffectConfigurableProductAttributes(4);
$objectManager1->create('Magento\ConfigurableProduct\Model\Product\Type\Configurable')->setUsedProductAttributeIds($attributes1, $product1);
$product1->setNewVariationsAttributeSetId(4); // Setting Attribute Set Id
$product1->setAssociatedProductIds($associatedProductIds);// Setting Associated Products
$product1->setCanSaveConfigurableAttributes(true);
$product1->save();
```
