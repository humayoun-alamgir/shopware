# Shopware 6 + Vue-Storefront

Clone the repo or install manually using the instructions below

---

Installation directory structure

    ~/www/shopware/shopware

    ~/www/shopware/pwa

## INSTALL SHOPWARE 6 (/www/shopware/**shopware**)

Github repo: [https://github.com/shopware/production](https://github.com/shopware/production)

    mkdir shopware

    git clone --branch=6.4.3.1 https://github.com/shopware/production shopware

Create mysql database named shopware then run

    composer install

    bin/console system:setup

    bin/console system:install --create-database --basic-setup

Now go to [http://shopware.local/admin](http://shopware.local/admin)

Login using

> Username: admin
> Password: shopware

## INSTALL PWA (VUE STOREFRONT PLUGIN)

[https://shopware-pwa-docs.vuestorefront.io/landing/getting-started/prepare-shopware.html#compatibility-table](https://shopware-pwa-docs.vuestorefront.io/landing/getting-started/prepare-shopware.html#compatibility-table)

Run

    composer require shopware-pwa/shopware-pwa

    bin/console plugin:refresh && bin/console plugin:install --activate SwagShopwarePwa

### INSTALL VUE STOREFRONT SERVER

[https://github.com/vuestorefront/shopware-pwa](https://github.com/vuestorefront/shopware-pwa)

    cd ..

    mkdir pwa

    cd pwa

    npx @shopware-pwa/cli@canary init

    yarn

    yarn dev

#### ERROR FIXING

Filter **Material** is not implemented! Please add new filter to `src/components/listing/types/Material.vue`

Change `/shopware-pwa/source/components/listing/SwProductListingFilter.vue` file, replace `getComponent()` function with:

    getComponent() {
        // 4/9/2021 - jewel - filter missing error fix
        //  - if file doesn't exist, use default filter
        //  - this will show new search attributes as checkboxs

        let fliterName = this.filterCode;
        try{
            require(`./types/${fliterName}.vue`);
        } catch (e) {
            fliterName = 'entity';
        }
        // -- end

        try {
            return () => ({
                component: import(
                    `@/components/listing/types/${fliterName}.vue`
                ),
                error: NoFilterFound,
            })
        } catch (e) {
            console.error("SwProductListingFilter:getComponent", e)
        }
    },

#### CUSTOMIZATIONS

Elastic search

Doc: https://developer.shopware.com/docs/guides/hosting/infrastructure/elasticsearch

Repo: https://github.com/shopware/elasticsearch
