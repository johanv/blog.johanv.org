.. title: How to change the status of a CiviCRM event participant by scanning a bar code.
.. slug: how-to-change-the-status-of-a-civicrm-event-participant-by-scanning-a-bar-code
.. date: 2016-05-27 22:12:09 UTC+02:00
.. tags: civicrm,api,howto
.. link: 
.. description: Here is the POC I promised at CiviCon NWEurope 2016.
.. type: text

Yesterday I was at `Civicon NWEurope 2016 <https://nweurope2016.civicrm.org/>`_.
I was talking to Yves, who was searching for a system that allowed him
to scan some kind of bar code of participants that turn up at an event, and
then automatically change the participants's status to 'Attended' in
CiviCRM.

.. TEASER_END

I told him this was not very difficult, and I promised to create a simple
proof of concept. So here it is.

I will just show that it is possible, in the most simple way. I created
a PHP application that reads participant ID's using a web form.
It calls CiviCRM using the API to change the status of those participants
to 'attended'. I chose to write a PHP application, because I assume that people
who use CiviCRM will understand PHP. But you can use any type of client.

So what you have to do, is send each participant a personalised letter or
e-mail with their participant ID in a bar code font (more details on
this later on). So with a scanner and the php app, we can change the
participant status.

This example works for CiviCRM with Drupal.

What to do in CiviCRM?
======================
To make this work, you need to create a Drupal user on your CiviCRM site, 
and make sure that this user has the following permissions in CiviCRM:

* Access CiviCRM
* Access CiviEvent
* Edit Event Participants
* View all CiviCRM contacts

(The last one is not strictly necessary, but my demo application shows
the names of the participants. If you want this, you need that permission.)

Your Drupal user will have a corresponding CiviCRM contact. You need to
`create an API key <https://wiki.civicrm.org/confluence/display/CRMDOC/REST+interface#RESTinterface-CreatingAPIkeysforusers>`_ for this contact.

The PHP client application
==========================
Here is the code for the application:

.. code-block:: php

    <html>
    <body>
    <?php
    // Copyright 2016 Chirojeugd Vlaanderen vzw
    // Licensed under the Apache License, Version 2.0
    // http://www.apache.org/licenses/LICENSE-2.0
    
    // WARNING: this is just a crappy example :-)
    // DO NOT USE THIS ON THE PUBLIC INTERNET!
    
    // This should work with CiviCRM 4.4 and Drupal 7.
    
    // You should create a Drupal user, that has the following permissions:
    //  * Access CiviCRM
    //  * Access CiviEvent
    //  * Edit Event Participants
    // Because I show the participant name, you also need
    //  * View all CiviCRM contacts
    
    // The corresponding CiviCRM contact should have an API key. I think this
    // can at the moment only be done using an SQL query:
    // UPDATE civicrm_contact SET API_KEY='your key' WHERE contact_id=XXX
    // (replace XXX by the CiviCRM contact ID of the corresponding contact, and
    // use a decent key.)
    
    // TODO: point to your own CiviCRM installation
    define('CIVI_URL', 'http://localhost/~johanv/civi-upstream');
    // TODO: change this to our site key;
    // search for SITE_KEY in sites/defaults/civicrm.settings.php.
    define('SITE_KEY', 'a4793695fad2b383a13e3a2dfa30d94d');
    // TODO: use the API key you assigned to your user
    define('API_KEY', '123 api');
    
    // CiviCRM things:
    define('STATUS_ATTENDED', 2);
    
    /**
     * Call CiviCRM HTTP API.
     *
     * @param $entity string Name of CiviCRM entity, e.g. Contact, Relationship,...
     * @param $action string 'get' to retrieve, 'create' to create or update
     * @param $params array parameters to be passed to the API
     */
    function _civicrm_http_api($entity, $action, $params) {
      // TODO: input validation. $action is the most important to check.
      $url = CIVI_URL . '/sites/all/modules/civicrm/extern/rest.php';
      $curl = curl_init();
      $rest_params = array(
          'key'=> SITE_KEY,
          'api_key' => API_KEY,
          'sequential' => 1,
          'entity' => $entity,
          'action' => $action,
          'json' => json_encode($params),
      );
    
      $opts = array(
          CURLOPT_URL => $url,
          CURLOPT_POST => TRUE,
          CURLOPT_POSTFIELDS => $rest_params,
          CURLOPT_RETURNTRANSFER => TRUE,
          CURLOPT_SSL_VERIFYPEER => FALSE,
      );
      curl_setopt_array($curl, $opts);
      $result = curl_exec($curl);
      curl_close($curl);
      $result = json_decode($result, true);
      // You should probably handle HTTP errors here.
      return $result;
    }
    
    if (isset($_POST['participantId'])) {
      $participantId = $_POST['participantId'];
      // avoid script injection.
      is_numeric($participantId) or die('Prutser!');
    
      // TODO: check whether the participant exists. (We are lucky,
      // because the API call will fail if the ID does not exist.)
      $params = array(
        // update participant with
        'id' => $participantId,
        // change status to 'attended'
        'status_id' => STATUS_ATTENDED,
        // Use chained call to get contact name
        'api.Contact.getsingle' => array(
          'id' => '$value.contact_id',
          'return' => 'display_name'
        ),
        // Use another chained call to get event detail
        'api.Event.getsingle' => array(
          'id' => '$value.event_id',
          'return' => 'title',
        ),
      );
      $result = _civicrm_http_api('Participant', 'create', $params);
      if ($result['is_error'] || $result['count'] != 1) {
        print "Something went wrong: <br />";
        print $result['error_message'] . "<br />";
      }
      else {
        $participant = $result['values'][0];
        print "Participant: " . $participant['api.Contact.getsingle']['display_name'] . "<br />";
        print "Event: " . $participant['api.Event.getsingle']['title'] . "<br />";
      }
    }
    
    ?>
      <form method="POST">
        <label for="participantId">Partipant ID:</label>
        <input name="participantId" />
        <input type="submit" />
      </form>
    <body>

You need to change the values of ``CIVI_URL``, ``SITE_KEY`` and
``API_KEY`` for your site. And again: this is just a demo. Do not put
it on the public internet, because it writes to your CiviCRM instance!

Generating the bar codes
========================
Now you still need to provide each of your participants with a bar code.
Easy. Just create a PDF letter for your participants, and use the
``{participant.participant_id}`` token. The bad news is that
CiviCRM does not provide tokens for participants
(`CRM-16734 <https://issues.civicrm.org/jira/browse/CRM-16734>`_). The
good news is that `a patch exists <https://patch-diff.githubusercontent.com/raw/civicrm/civicrm-core/pull/8260.diff>`_. (Not sure how to apply a patch?
See :doc:`How to apply a patch <how-to-apply-a-patch>`.)

If you have applied the patch, you can use the
``{participant.participant_id}`` token, but you'll have to type it into
your template, because it is not available from the drop down list.

Then you should figure out how to use a barcode font in your PDF
letter. I have not looked into this yet, but I guess it should
be doable.

Thoughts
========
I cannot repeat enough that this is just a proof of concept. It read
participant IDs, and changes statuses. Do not use this to prevent people
from entering your event without paying; it is very easy to spoof the
bar code by guessing other participant ID's.

If you do not want to patch CiviCRM, you can also generate a bar code
based on contact ID and event ID. You will have to adapt your API calls,
but it will not be too hard. I wanted to use the participant ID because
it keeps things simple, and of course I also wanted to promote
`CRM-16734 <https://issues.civicrm.org/jira/browse/CRM-16734>`_ ;-)

