---
title: Customization example - Suite Product Details page
description: Customize any front–end element in Spryker by overriding a respective SCSS file.
last_updated: Jun 16, 2021
template: howto-guide-template
originalLink: https://documentation.spryker.com/2021080/docs/customization-example-suite-product-details-page
originalArticleId: 748f6c93-de3a-4f69-94ba-5899ec8be494
redirect_from:
  - /2021080/docs/customization-example-suite-product-details-page
  - /2021080/docs/en/customization-example-suite-product-details-page
  - /docs/customization-example-suite-product-details-page
  - /docs/en/customization-example-suite-product-details-page
  - /v6/docs/customization-example-suite-product-details-page
  - /v6/docs/en/customization-example-suite-product-details-page
---

In Spryker, front-end elements have dedicated SCSS styles. To show you how to customize the Spryker front end, we broke down the *Product Details* page from our [Spryker Suite](https://github.com/spryker-shop/suite) into separate elements with their respective style files. To customize a particular element, you [override it with the desired code](/docs/scos/dev/front-end-development/yves/atomic-frontend/managing-the-components/overriding-a-component.html) in the respective style file.

![suite-1](https://spryker.s3.eu-central-1.amazonaws.com/docs/Developer+Guide/Development+Guide/Front-End/Yves/Atomic+Frontend/%D0%A1ustomization+example+-+Suite+Product+Details+page/suite-1.png)


| # | PATH TO SCSS |
| --- | --- |
| 1 | project/src/Pyz/Yves/ShopUi/Theme/default/components/molecules/breadcrumb-step/breadcrumb-step.scss |
| 2 | project/vendor/spryker-shop/price-product-volume-widget/src/SprykerShop/Yves/PriceProductVolumeWidget/Theme/default/components/molecules/volume-price/volume-price.scss |
| 3 | project/vendor/spryker-shop/product-review-widget/src/SprykerShop/Yves/ProductReviewWidget/Theme/default/components/molecules/rating-selector/rating-selector.scss |
| 4 | project/vendor/spryker-shop/product-group-widget/src/SprykerShop/Yves/ProductGroupWidget/Theme/default/components/molecules/product-detail-color-selector/product-detail-color-selector.scss |
| 5 | project/vendor/spryker-shop/shop-ui/src/SprykerShop/Yves/ShopUi/Theme/default/components/atoms/select/select.scss |
| 6 | project/vendor/spryker-shop/shop-ui/src/SprykerShop/Yves/ShopUi/Theme/default/components/atoms/button/button.scss |

![suite-2](https://spryker.s3.eu-central-1.amazonaws.com/docs/Developer+Guide/Development+Guide/Front-End/Yves/Atomic+Frontend/%D0%A1ustomization+example+-+Suite+Product+Details+page/suite-2.png)

| # | PATH TO SCSS |
| --- | --- |
| 3 | project/vendor/spryker-shop/product-review-widget/src/SprykerShop/Yves/ProductReviewWidget/Theme/default/components/molecules/rating-selector/rating-selector.scss |
| 7 | project/vendor/spryker-shop/shop-ui/src/SprykerShop/Yves/ShopUi/Theme/default/components/molecules/pagination/pagination.scss |
