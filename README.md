# ICUBE SWIFT Hyvä
This is SWIFT Hyvä code base using Magento CE 2.4.6 PHP 8.1

Installation:
============================================================
- clone https://github.com/icube-mage/swift-hyva
- go to develop branch 
- run composer install --prefer-dist
- npm install @tailwindcss/line-clamp
- cd vendor/icubebysirclo/modules-swift-hyva/src/theme-frontend-swift-hyva/web/tailwind && npm install @hyva-themes/hyva-modules
- Run ​​project-patch.sh
- install using terminal: php bin/magento setup:install --cleanup-database --base-url=http://local.magehyva.com/ --base-url-secure=https://local.magehyva.com/ --db-host=127.0.0.1 --db-name=magehyva --db-user=mage2hyva --db-password=mage2hyva --admin-firstname=Hyva --admin-lastname=Install --admin-email=sysadmin@icube.us --admin-user=mage2user --admin-password=Password123 --backend-frontname=backoffice --language=en_US --currency=IDR --timezone=Asia/Jakarta --use-rewrites=1 --use-secure-admin=1
- after installation is completed, Full deploy then change your theme to Swift Hyva theme ( open BO > Content > configuration)

If Swift Hyva theme still not installed:
============================================================
- composer config repositories.icube-bysirclo-hyva git git@github.com:icubebysirclo/modules-swift-hyva.git
- composer require icubebysirclo/modules-swift-hyva:dev-master -vvv
  
Setup sample data (If you want to install with sample data):
============================================================
- run php bin/magento deploy:mode:set developer
- run php bin/magento sampledata:deploy << setup sample data, optional
- run php bin/magento setup:upgrade && php bin/magento setup:di:compile && php bin/magento setup:static-content:deploy -f && php bin/magento cache:flush
- in local run chmod -R 777 var/ pub/ generated/

# Disable OTP:
============================================================
- bin/magento module:disable Icube_CustomSmsOtpWithGraphQl Icube_OtpFazpass Icube_SmsOtp Icube_SmsOtpGraphQl Icube_Telephone Icube_Twilio
- bin/magento s:d:c

# Disable VES menu:
============================================================
- bin/magento module:disable Icube_Vesmegamenu Ves_All Ves_Megamenu Ves_Setup
- bin/magento s:d:c

Additional to disable 2FA in local:
============================================================
- bin/magento module:disable Magento_AdminAdobeImsTwoFactorAuth Magento_TwoFactorAuth
- bin/magento s:d:c

Deployment Script
=============================================================
## Local:
Always run `npm run watch` in `app/design/frontend/Swift/hyva/web/tailwind`
### Full Deploy
```
generated/* pub/static/frontend/* pub/static/_cache/merged pub/static/_requirejs pub/static/adminhtml
php bin/magento setup:upgrade
php bin/magento setup:di:compile
php bin/magento hyva:config:generate
php bin/magento setup:static-content:deploy en_US id_ID --jobs=5 -f
php bin/magento cache:clean
chmod -R 777 var/ pub/ generated/
```
### FE deploy
```
rm -rf var/cache var/composer_home var/di var/generation var/page_cache var/session var/tmp var/view_preprocessed pub/static/* generated/* pub/static/frontend/* pub/static/_cache/merged pub/static/_requirejs pub/static/adminhtml
php bin/magento hyva:config:generate
php bin/magento setup:static-content:deploy en_US id_ID --jobs=5 -f
php bin/magento cache:clean
chmod -R 777 var/ pub/ generated/
```

## Dev Site
```
rm -rf var/cache var/composer_home var/di var/generation var/page_cache var/session var/tmp var/view_preprocessed pub/static/* generated/* pub/static/frontend/* pub/static/_cache/merged pub/static/_requirejs pub/static/adminhtml
php bin/magento setup:upgrade
php bin/magento setup:di:compile
php bin/magento cache:clean
php bin/magento hyva:config:generate
npm --prefix app/design/frontend/Swift/hyva/web/tailwind run build-prod
php bin/magento setup:static-content:deploy en_US id_ID --jobs=5 -f
php bin/magento c:f
sudo service varnish restart
```