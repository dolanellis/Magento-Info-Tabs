Magento-Info-Tabs
=================

Add  Tabs to Magento Tabs


####Place this in Your local.xml file:

```xml
<!--
Catalog Product View Layout
-->
<?xml version="1.0"?>
<layout>
  <catalog_product_view translate="label">
	<reference name="content">
			<reference name="product.info">
				<block type="catalog/product_view_tabs" name="product.info.tabs" as="info_tabs" template="catalog/product/view/tabs.phtml" >
					<action method="addTab" translate="title" module="catalog"><alias>description</alias><title>Product Description</title><block>catalog/product_view_description</block><template>catalog/product/view/description.phtml</template></action>
					<action method="addTab" translate="title" module="catalog"><alias>upsell_products</alias><title>We Also Recommend</title><block>catalog/product_list_upsell</block><template>catalog/product/list/upsell.phtml</template></action>
					<action method="addTab" translate="title" module="catalog"><alias>additional</alias><title>Additional Information</title><block>catalog/product_view_attributes</block><template>catalog/product/view/attributes.phtml</template></action>
					<!-- Single Attribute Tab -->
					<action method="addTab" translate="title" module="catalog"><alias>example_alias</alias><title>Example_Title</title><block>catalog/product_view_attributes</block><template>catalog/product/view/example.phtml</template></action>
					<!-- Static block Tab -->
					<action method="addTab" translate="title" module="catalog"><alias>static_block_name</alias><title>Warranty</title><block>cms/block</block><template>null</template></action>
                              		<!-- define the CMS block ID for the shipping info tab -->
					<block type="cms/block" name="product.info.tabs.static_block_name" as="static_block_name">
						<action method="setBlockId"><block_id>static_block_name</block_id></action>
					</block>
				</block>       
			</reference>
		</reference>
	</catalog_product_view>  
</layout>
```

####For Single Attribute Tabs Create example.phtml in /YourTheme/template/catalog/product/view/example.phtml

```phtml
<?php $_product = $this->getProduct() ?>
<?php echo $_product->getData('example_attribute'); ?>
```

####Create "tabs.phtml" file in (/YourTemplate/catalog/product/view/tabs.phtml) using the code below:

```php
<?php
/**
 * Magento
 *
 * NOTICE OF LICENSE
 *
 * This source file is subject to the Academic Free License (AFL 3.0)
 * that is bundled with this package in the file LICENSE_AFL.txt.
 * It is also available through the world-wide-web at this URL:
 * http://opensource.org/licenses/afl-3.0.php
 * If you did not receive a copy of the license and are unable to
 * obtain it through the world-wide-web, please send an email
 * to license@magentocommerce.com so we can send you a copy immediately.
 *
 * DISCLAIMER
 *
 * Do not edit or add to this file if you wish to upgrade Magento to newer
 * versions in the future. If you wish to customize Magento for your
 * needs please refer to http://www.magentocommerce.com for more information.
 *
 * @category    design
 * @package     default_modern
 * @copyright   Copyright (c) 2012 Magento Inc. (http://www.magentocommerce.com)
 * @license     http://opensource.org/licenses/afl-3.0.php  Academic Free License (AFL 3.0)
 */

/**
 * Product view template
 *
 * @see Mage_Catalog_Block_Product_View
 */
?>
<ul class="product-tabs">
    <?php foreach ($this->getTabs() as $_index => $_tab): ?>
        <?php if($this->getChildHtml($_tab['alias'])): ?>
            <li id="product_tabs_<?php echo $_tab['alias'] ?>" class="<?php echo !$_index?' active first':(($_index==count($this->getTabs())-1)?' last':'')?>"><a href="#"><?php echo $_tab['title']?></a></li>
        <?php endif; ?>
    <?php endforeach; ?>
</ul>
<?php foreach ($this->getTabs() as $_index => $_tab): ?>
    <?php if($this->getChildHtml($_tab['alias'])): ?>
        <div class="product-tabs-content" id="product_tabs_<?php echo $_tab['alias'] ?>_contents"><?php echo $this->getChildHtml($_tab['alias']) ?></div>
    <?php endif; ?>
<?php endforeach; ?>
<script type="text/javascript">
//<![CDATA[
Varien.Tabs = Class.create();
Varien.Tabs.prototype = {
  initialize: function(selector) {
    var self=this;
    $$(selector+' a').each(this.initTab.bind(this));
  },

  initTab: function(el) {
      el.href = 'javascript:void(0)';
      if ($(el.parentNode).hasClassName('active')) {
        this.showContent(el);
      }
      el.observe('click', this.showContent.bind(this, el));
  },

  showContent: function(a) {
    var li = $(a.parentNode), ul = $(li.parentNode);
    ul.select('li', 'ol').each(function(el){
      var contents = $(el.id+'_contents');
      if (el==li) {
        el.addClassName('active');
        contents.show();
      } else {
        el.removeClassName('active');
        contents.hide();
      }
    });
  }
}
new Varien.Tabs('.product-tabs');
//]]>
</script>

```

####Add This to /YourTemplate/catalog/product/view.phtml in the area you would like you Info Tabs to show up

```php
<?php echo $this->getChildHtml('info_tabs');?>
```
