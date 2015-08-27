.. title: Exploding a string with value separators to an array in CiviCRM
.. slug: exploding-a-string-with-value-separators-to-an-array-in-civicrm
.. date: 2015-08-27 13:16:22 UTC+02:00
.. tags: civicrm
.. category: 
.. link: 
.. description: 
.. type: text

CiviCRM stores its multiselect custom field values in the database
by concatenating the values, e.g. like this ::

  *value1*value2*

(I replaced the CRM_Core_DAO::VALUE_SEPARATOR by * because the real unicode
character, #&01, does not show anything sensible.)

This represents ::

  (
      [0] => value1
      [1] => value2
  )

Now if you just use :code:`explode(CRM_Core_DAO::VALUE_SEPARATOR, $value)`, you
get ::

  (
      [0] =>
      [1] => value1
      [2] => value2
      [3] =>
  )

This is probably not wat you want. What you really want to do, is ::

  CRM_Utils_Array::explodePadded($value)

I just put this in a blog post, because I always forget, and Google doesn't
seem to know.

