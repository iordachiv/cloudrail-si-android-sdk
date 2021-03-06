<p align="center">
  <img width="200px" src="http://cloudrail.github.io/img/cloudrail_logo_github.png"/>
</p>
# CloudRail SI for Android
Integrate Multiple Services With Just One API

<p align="center">
  <img width="300px" src="http://cloudrail.github.io/img/cloudrail_si_github.png"/>
</p>

CloudRail is a free software library which abstracts multiple APIs from different providers into a single and universal interface.

**Current Interfaces:**
<p align="center">
  <img width="800px" src="http://cloudrail.github.io/img/available_interfaces_v2.png"/>
</p>

Full documentation can be found at https://docs.cloudrail.com/

With CloudRail, you can easily integrate external APIs into your application. CloudRail is an abstracted interface that takes several services and then gives a developer-friendly API that uses common functions between all providers. This means that, for example, upload() works in exactly the same way for Dropbox as it does for Google Drive, OneDrive, and other Cloud Storage Services, and getEmail() works similarly the same way across all social networks.

## Current Interfaces
Interface | Included Services 
--- | --- 
Cloud Storage | Dropbox, Google Drive, OneDrive, Box
Social Profiles | Facebook, GitHub, Google+, LinkedIn, Slack, Twitter, Windows Live, Yahoo, Instagram
Payment | PayPal, Stripe
Email | Maljet, Sendgrid
SMS | Twilio, Nexmo
Point of Interest | Google Places, Foursquare, Yelp
---
### Cloud Storage Interface:

* Dropbox
* Box
* Google Drive
* Microsoft OneDrive

#### Features:

* Download files from Cloud Storage.
* Upload files to Cloud Storage.
* Get Meta Data of files, folders and perform all standard operations (copy, move, etc) with them.
* Retrieve user information.

[Full Documentation](https://docs.cloudrail.com/docs/cloud-storage)
#### Code Example:

```` java
// CloudStorage cs = new Box(context, "[clientIdentifier]", "[clientSecret]");
// CloudStorage cs = new OneDrive(context, "[clientIdentifier]", "[clientSecret]");
// CloudStorage cs = new GoogleDrive(context, "[clientIdentifier]", "[clientSecret]");
CloudStorage cs = new Dropbox(context, "[clientIdentifier]", "[clientSecret]");
new Thread() {
    @Override
    public void run() {
        cs.createFolder("/TestFolder"); // <---
        InputStream stream = null;
        try {
            AssetManager assetManager = getAssets();
            stream = assetManager.open("UserData.csv");
            long size = assetManager.openFd("UserData.csv").getLength();
            cs.upload("/TestFolder/Data.csv", stream, size, false); // <---
        } catch (Exception e) {
            // TODO: handle error
        } finally {
            // TODO: close stream
        }
    }
}.start();
````
---
### Social Media Profiles Interface:

* Facebook
* Github
* Google Plus
* LinkedIn
* Slack
* Twitter
* Windows Live
* Yahoo
* Instagram

#### Features

* Get profile information, including full names, emails, genders, date of birth, and locales.
* Retrieve profile pictures.
* Login using the Social Network.

[Full Documentation](https://docs.cloudrail.com/docs/profile)
#### Code Example:

```` java
// final Profile profile = new GooglePlus(this, "[clientIdentifier]", "[clientSecret]");
// final Profile profile = new GitHub(this, "[clientIdentifier]", "[clientSecret]");
// final Profile profile = new Slack(this, "[clientIdentifier]", "[clientSecret]");
// final Profile profile = new Instagram(this, "[clientIdentifier]", "[clientSecret]");
// ...
final Profile profile = new Facebook(this, "[clientIdentifier]", "[clientSecret]");
new Thread() {
    @Override
    public void run() {
        String fullName = profile.getFullName();
        String email = profile.getEmail();
        // ...
    }
}.start();
````
---
### Payment Interface:

* PayPal
* Stripe

#### Features

* Perform charges
* Refund previously made charges
* Manage subscriptions

[Full Documentation](https://docs.cloudrail.com/docs/payment)
#### Code Example

```` java
// final Payment payment = new Stripe(this, "[secretKey]");
final Payment payment = new PayPal(this, true, "[clientIdentifier]", "[clientSecret]");
new Thread() {
    @Override
    public void run() {
        CreditCard source = new CreditCard(null, 6L, 2021L, "xxxxxxxxxxxxxxxx", "visa", "<FirstName>", "<LastName>", null);
        Charge charge = payment.createCharge(500L, "USD", source);
        Log.i("info", "Charge: " + charge.toString());    
    }
}.start();
````
---
### Email Interface:

* Mailjet
* Sendgrid

#### Features

* Send Email

[Full Documentation](https://docs.cloudrail.com/docs/email)

#### Code Example

````java
// final Email email = new Mailjet(this, "[clientID]", "[clientSecret]");
final Email email = new Sendgrid(this, "[username]", "[password]");
new Thread() {
    @Override
    public void run() {
        email.sendEmail("info@cloudrail.com", "CloudRail", Arrays.asList("foo@bar.com", "bar@foo.com"), "Welcome", "Hello from CloudRail", null, null, null);
    }
}.start();
````
---
### SMS Interface:

* Twilio
* Nexmo

#### Features

* Send SMS

[Full Documentation](https://docs.cloudrail.com/docs/sms)

#### Code Example

````java
// final SMS sms = new Nexmo(this, "[clientIdentifier]", "[clientSecret]");
final SMS sms = new Twilio(this, "[clientIdentifier]", "[clientSecret]");
new Thread() {
@Override
    public void run() {
        sms.sendSMS("CloudRail", "+4912345678", "Hello from CloudRail");
    }
}.start();
````
---
### Points of Interest Interface:

* Google Places
* Foursquare
* Yelp

#### Features

* Get a list of POIs nearby
* Filter by categories or search term

[Full Documentation](https://docs.cloudrail.com/docs/points-of-interest)
#### Code Example

```` java
// final PointsOfInteres poi = new Foursquare(this, "[clientID]", "[clientSecret]");
// final PointsOfInteres poi = new Yelp(this, "[consumerKey]", "[consumerSecret]", "[token]", "[tokenSecret]");
final PointsOfInteres poi = new GooglePlaces(this, "[apiKey]");
new Thread() {
    @Override
    public void run() {
        List<POI> res = poi.getNearbyPOIs(49.4557091, 8.5279138, 1000L, "restaurant", null);
        Log.i("info", "POIs: " + res.toString());    
    }
}.start();
````
---
More interfaces are coming soon.

## Advantages of Using CloudRail

* Consistent Interfaces: As functions work the same across all services, you can perform tasks between services simply.

* Easy Authentication: CloudRail includes easy ways to authenticate, to remove one of the biggest hassles of coding for external APIs.

* Switch services instantly: One line of code is needed to set up the service you are using. Changing which service is as simple as changing the name to the one you wish to use.

* Simple Documentation: There is no searching around Stack Overflow for the answer. The CloudRail documentation at https://docs.cloudrail.com/ is regularly updated, clean, and simple to use.

* No Maintenance Times: The CloudRail Libraries are updated when a provider changes their API.

* Direct Data: Everything happens directly in the Library. No data ever passes a CloudRail server.

## Maven
build.gradle
````
repositories {
    maven {
        url "http://maven.cloudrail.com"
    }
}

dependencies {
    compile 'com.cloudrail:cloudrail-si-android:2.4.2'
}
````

## Get Updates

To keep updated with CloudRail, including any new providers that are added, just add your email address to https://cloudrail.com/updates/.

## Pricing

CloudRail is **free to use**. For all projects; **commercial and non-commercial**. We want APIs to be accessible to all developers. We want APIs to be easier to manage. This is only possible if there are free, powerful tools out there to do this. The only favor we are asking for, is to spread the word about this library. Thank you!

CloudRail also has enterprise licensing options. Please contact us for more information: support@cloudrail.com

## Other Platforms

CloudRail is also available for other platforms like iOS and Java. You can find all libraries on https://cloudrail.github.io

## Questions?

Get in touch at any time by emailing us: support@cloudrail.com 

or

Tag a question with cloudrail on [StackOverflow](http://stackoverflow.com/questions/tagged/cloudrail)
