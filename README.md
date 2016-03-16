# Implement Facebook Login on Your Website
This is a tutorial that shows how to create communication between a third party service and a database.

What you need:

1. PHP version 5.4 or greater
    >To check what version of php you have, you can go into the public folder of your slim application, and create a file in
    >zero-to-slim.dev -> public folder called test.php with the following code:
  ```
      <?php
        echo phpversion();
      ?>
  ```

   >Save, and then go to zero-to-slim.dev/test.php and it will tell you what version you have
   
2. Have PHP mbstring Enabled
    >To check that you have this enabled run the following and it should be listed
  ```
     php -m 
  ```

After you have met the basic requirements, you need to register your app with Facebook. To do this, go to ```developers.facebook.com```
and log in. 

Once you are logged in, you will have a menu called **My Apps**. Click that and create a new app.


INSERT PICTURE HERE


In this tutorial we will be creating a web application. When prompted, choose the website option and create a name for your app. For the app category you can choose whatever you want.


INSERT PICTURE HERE


Click **create App ID**. Once you have done that, click **My Apps** again and choose your app from the menu. You will be taken to a dashboard that shows you your app ID, version, and app secret (digital certificate). You will need these for later. For now, click on **settings** in the left menu bar.


INSERT PICTURE HERE


You should see this:


INSERT PICTURE HERE


Under ```App Domains```, type in the URL of your website and click **Add Platform**. Insert the same URL and click save changes.


INSERT PICTURE HERE


The next step is to download the PHP SDK that Facebook provides. To do this, go to the link posted below and click to manually install SDK for PHP.

```https://developers.facebook.com/docs/php/gettingstarted                                                            ```
