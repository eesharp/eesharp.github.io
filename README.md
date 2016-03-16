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


 
![pic1](https://cloud.githubusercontent.com/assets/17342721/13798818/cb064408-eae7-11e5-8eb2-0575f2d1786d.png)

 

In this tutorial we will be creating a web application. When prompted, choose the website option and create a name for your app. For the app category you can choose whatever you want.

 
 
![pic2](https://cloud.githubusercontent.com/assets/17342721/13798822/cd2fc4e8-eae7-11e5-8a19-29e86f43ad3e.png)
 
 

Click **create App ID**. Once you have done that, click **My Apps** again and choose your app from the menu. You will be taken to a dashboard that shows you your app ID, version, and app secret (digital certificate). You will need these for later. For now, click on **settings** in the left menu bar.


 
![pic3](https://cloud.githubusercontent.com/assets/17342721/13798823/ce46fbd0-eae7-11e5-9f70-30919903c220.png)

 

You should see this:


 
![pic4](https://cloud.githubusercontent.com/assets/17342721/13798826/cf79f23c-eae7-11e5-80a1-35809fd74113.png)

 

Under ```App Domains```, type in the URL of your website and click **Add Platform**. Insert the same URL and click save changes.


 
![pic5](https://cloud.githubusercontent.com/assets/17342721/13798827/d0638a14-eae7-11e5-8752-4e6ee4b5c083.png)

 

The next step is to download the PHP SDK that Facebook provides. To do this, follow the next steps to manually install SDK for PHP.


 
![pic6](https://cloud.githubusercontent.com/assets/17342721/13798828/d150a632-eae7-11e5-9c6d-075cf5861ed3.png)

 

After you unzip the file, it should look like this:


 
![pic7](https://cloud.githubusercontent.com/assets/17342721/13798829/d2450718-eae7-11e5-981f-f640b41d69ff.png)

 

In your ```zero-to-slim.dev/public``` folder, I would recommend that you create a ```fbtest``` file


 
![pic8](https://cloud.githubusercontent.com/assets/17342721/13798830/d384756e-eae7-11e5-9359-df6e6e823fe9.png)

 

Then copy the ```src``` folder from the facebook file you downloaded and paste it into the ```fbtest``` file.


 
![pic9](https://cloud.githubusercontent.com/assets/17342721/13798831/d48e4228-eae7-11e5-8eea-12da63d97171.png)

 

In the same fbtest file, create a file called ```index.php```. 
As for the code that goes in this file, copy and paste the following into your index.php file:

```
<?php
session_start();
require_once __DIR__ . '/src/Facebook/autoload.php';

$fb = new Facebook\Facebook([
  'app_id' => 'APP_ID',
  'app_secret' => 'APP_SECRET',
  'default_graph_version' => 'v2.4',
  ]);

$helper = $fb->getRedirectLoginHelper();

$permissions = ['email']; // optional
	
try {
	if (isset($_SESSION['facebook_access_token'])) {
		$accessToken = $_SESSION['facebook_access_token'];
	} else {
  		$accessToken = $helper->getAccessToken();
	}
} catch(Facebook\Exceptions\FacebookResponseException $e) {
 	// When Graph returns an error
 	echo 'Graph returned an error: ' . $e->getMessage();

  	exit;
} catch(Facebook\Exceptions\FacebookSDKException $e) {
 	// When validation fails or other local issues
	echo 'Facebook SDK returned an error: ' . $e->getMessage();
  	exit;
 }

if (isset($accessToken)) {
	if (isset($_SESSION['facebook_access_token'])) {
		$fb->setDefaultAccessToken($_SESSION['facebook_access_token']);
	} else {
		// getting short-lived access token
		$_SESSION['facebook_access_token'] = (string) $accessToken;

	  	// OAuth 2.0 client handler
		$oAuth2Client = $fb->getOAuth2Client();

		// Exchanges a short-lived access token for a long-lived one
		$longLivedAccessToken = $oAuth2Client->getLongLivedAccessToken($_SESSION['facebook_access_token']);

		$_SESSION['facebook_access_token'] = (string) $longLivedAccessToken;

		// setting default access token to be used in script
		$fb->setDefaultAccessToken($_SESSION['facebook_access_token']);
	}

	// redirect the user back to the same page if it has "code" GET variable
	if (isset($_GET['code'])) {
		header('Location: ./');
	}

	// getting basic info about user
	try {
		$profile_request = $fb->get('/me?fields=name,first_name,last_name,email');
		$profile = $profile_request->getGraphNode()->asArray();
	} catch(Facebook\Exceptions\FacebookResponseException $e) {
		// When Graph returns an error
		echo 'Graph returned an error: ' . $e->getMessage();
		session_destroy();
		// redirecting user back to app login page
		header("Location: ./");
		exit;
	} catch(Facebook\Exceptions\FacebookSDKException $e) {
		// When validation fails or other local issues
		echo 'Facebook SDK returned an error: ' . $e->getMessage();
		exit;
	}
	
	// printing $profile array on the screen which holds the basic info about user
	print_r($profile);

  	// Now you can redirect to another page and use the access token from $_SESSION['facebook_access_token']
} else {
	// replace your website URL same as added in the developers.facebook.com/apps e.g. if you used http instead of https and you used non-www version or www version of your website then you must add the same here
	$loginUrl = $helper->getLoginUrl('https://sohaibilyas.com/fbapp/', $permissions);
	echo '<a href="' . $loginUrl . '">Log in with Facebook!</a>';
}
```

There are a couple of things you need to change in this file.
>First, at the top you need to enter your **App ID**, **App Secret**, and change the version to **2.5**.

```
<?php
	session_start();
	require_once __DIR__ . '/src/Facebook/autoload.php';

	$fb = new Facebook\Facebook([
  	'app_id' => 'APP_ID',
 	'app_secret' => 'APP_SECRET',
  	'default_graph_version' => 'v2.4',
 	 ]);
```

>Then, scroll down to the bottom. You will need to replace the url with your own website.

>replace your website URL same as added in the ```developers.facebook.com/apps``` e.g. if you used ```http``` instead of ```https``` and you used ```non-www``` version or ```www version``` of your website, then you must add the same here:

```
$loginUrl = $helper->getLoginUrl('http://zero-to-slim.dev/fbtest/index.php', $permissions);
    echo '<a href="' . $loginUrl . '">Log in with Facebook!</a>';
    }
```

Now there is one last thing to do. You need to add the following to the ```routes.php``` file in your ```zero-to-slim.dev``` directory so that ```/fbtest/``` will automatically redirect to ```/fbtest/index.php```.


```
$app->get('/fbtest/', function ($request, $response, $args){
  $url = 'http://zero-to-slim.dev/fbtest/index.php';
  header('Location: index.php');
  exit();
});
```

Go to ```http://zero-to-slim.dev/fbtest/index.php``` and you will see a link allowing you to log into Facebook.




```https://developers.facebook.com/docs/php/gettingstarted                                                            ```


