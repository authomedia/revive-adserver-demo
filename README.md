# Ad Server Example with Revive-Adserver

Quick example of a Revive Ad Server serving up a campaign

## Prerequisites

- Docker
- Docker Compose
- Ruby `asdf` or another "instant" web server (feel free to Dockerize this!)

## Setting up

* First, update the docker-compose.yml with an appropriate `MYSQL_ROOT_PASSWORD`
* Next, start your docker stack with `docker-compose up`
* Navigate to http://localhost:8080
* Log into the adminer interface. Use `database` as the host and `root` for the user. Enter the password you put into the `docker-compose.yml` file to access the database
* Use the (retro!) UI to create a new database called `revive1`. Use `utf8_general_ci` encoding!
* Create a MySQL user to run Revive under and give it permission to modify the `revive` database
* Next navigate to http://localhost:5000 and follow the steps to configure your Revive Adserver instance. Enter the database url as `database` and enter the database login details of the user you created (or `root` if you are feeling lazy/unsafe)
* After a few moments you will have a Revive Adserver instance up and running. Click "Continue" to login and access it

### Creating a new Campaign

* First, click on "Advertisers" and create a simple Advertiser profile. This acts as a container for all the advertising for that Advertiser
* Next click on "Campaigns > Add New Campaign". Enter a name, choose "Remnant" campaign type. Enter a price and set the campaign weight to "10". Save Changes.

### Creating a new Banner

* Click on "Banners > Add New Banner"
* Choose "Generic HTML Banner"
* Paste the code from `banner.html` in this project root to create a new banner based on the XBox Interactive IFRAME
* **IMPORTANT**: Untick the "This banner can safely be displayed in an IFRAME". We need to do this to allow it to be responsive
* Enter Size: 900x420 in the fields provided. The width/height are usually used to target specific zones but our zones are responsive, so this is unimportant, but still needs some values
* Click "Save Changes"

### Creating a new Websites

* Click on "Websites > Add New Website"
* Enter "localhost" and some other basic information
* Click on "Zones > Add new Zone"
* Enter `*` x `*`  in the "Size" field. All other fields can be left as defaults
* Click on the new Zone you have created
* Click the "Linked Banners" tab
* Use the select boxes to select your Advertiser and Camapaign then click the (>) button to add your campaign to the Linked Banners
* Click the "Probability" tab to see your banner's delivery rate
* Click the "Invocation Code" link, then copy the Bannercode field to the clipboard

### Delivering your ad

* Open the `index.html` page in the project
* Find the section `<!-- PASTE YOUR AD TAG HERE -->`
* Paste in the ad banner code you copied previously
* Modify the copied ad tag to include `style="width:100%"` on the <ins> tag otherwise your ad will not be responsive!
* From the terminal run `bundle install` to install `asdf`
* Start the `asdf` ad server by typing `asdf` into the terminal
* Navigate to the demo webpage at http://localhost:9292

### Lowering the cache settings

For testing you should lower the caching settings to ensure you can see changes to your campaign right away

* In Revive UI, click the "Working as: Default Manager" dropdown in the top right of the UI
* Choose "Switch To: Administrator Account"
* Click the "Configuration" tab
* Under "Banner Delivery Settings" set the "Time Between Banner Cache Updates (seconds)" to "10"
* Save Changes

Now refresh the ad test page and you should see your campaign delivering in the middle of the site


## Configuring a 3rd-party tag test

To configure a 3rd-party tag, repeat the above steps for the Revive Adserver running on http://localhost:5001 with the following changes:

* When creating the HTML Banner, copy the "BannerCode" you pasted into the `index.html` into the "Create an HTML banner - banner code" field of the New Banner page. Be sure to include the `width:100%` change on the `<ins>` tag to make it responsive.
* To test multiple ads delivering into the same zone, create another ad using a different IFRAME url, and ensure both are linked to the same zone
* Paste the "Invocation Code" from the 2nd Revive Adserver's zone into your index.html, and reload the test page. You should see the 2 banners on rotation





