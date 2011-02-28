Overview
========

CloudKnock is an application for co-working facilities. It lets co-working members easily manage checkins and checkouts to their facility without the need for any specialized hardware, or by broadcasting their location on a social network.

When a member arrives at a co-working facility and needs to be let in, they simple send the message "knock knock" on one of the channels supported by the Tropo platform.  A message is then sent to all currently checked in co-working members that there is someone "knocking" on the door..

Its simple, fast and easy - never be locked out of your co-working site again!

Set Up
======

Simply create a new [Tropo Scripting](https://www.tropo.com/docs/scripting/overview.htm) application, and load in the contents of the index.php file.  Add the details of your CouchDB instance to the constant declarations at the top of the script:

    // The details of your CouchDB instance.
    define("COUCH_DB_HOST", "http://user:passd@couchdb.host.com");
    define("COUCH_DB_PORT", "5984");
    define("COUCH_DB_NAME", "cloudknock"); 

Creating Users
==============

There is no GUI or front-end provided for user creation as part of this solution - this process is completely customizable and "skinable" by those wishing to use CloudKnock.  

Create any front end for user management that you like - the process of creating and adding users is simple.  

To add a user, simply do an HTTP POST to your CouchDB instance with JSON conforming to that generated by the user class (included in this solution).  For an example of how to do this, see the sample.php file.


    <?php
    
    require 'classes/user.php';
    require 'path/to/Sag.php';
    
    // Add details for the user.
    $user = new User(null, null, null, null, "5551112222", null, "15551112222", null);
    $user->setName("Your Name");
    
    // insert the user into your CouchDB instance.
    $sag = new Sag('http://user:passd@couchdb.host.com', '5984');
    $sag->setDatabase('cloudknock');
    $sag->post(sprintf($user));
    
    ?>

To update a user, simply do an HTTP PUT, to delete just use an HTTP DELETE.  It's that simple.

For more information on managing documents in a CouchDB instance, refer to the [CouchDB wiki](http://wiki.apache.org/couchdb/HTTP_Document_API).

Usage
=====

To check in to a co-working facility, an established user simply sends "check in" on the channel they wish to utilize.  This channel must be one that is set up in their user record in the CouchDB database (e.g., if they wish to send a test message, they must have an SMS number in their user record).

To check out, a user simply sends "check out".

A user outside a co-working facility can send a message to all checked in users inside the facility by simply sending "knock knock".  

Only established users may send a "virtual knock" to checked in users. 