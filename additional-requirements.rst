.. _additional-requirements:

Additional Requirements
=======================

Nowadays, RUNALYZE comes with a lot of advanced features that require several
micro services. Most of them are required for some special feature only or
provide optional enhancements. It's your own responsibility to get them working.


Permissions
-----------

The following directories need to be writable:

* data/cache/
* data/import/
* data/log/
* data/poster/
* data/sessions/
* var/cache/
* var/logs/
* var/tmp/


.. _queueing-system:

Queue
-----

*From v4.0 on*

As the complexity of RUNALYZE arises, not all requests can be handled within a
few seconds. A queueing system is used to process long lasting tasks (e.g.
generating a complete backup or fancy posters from all your activities) in
background.

To process the queue, you need to create a cronjob for::

   runalyze:queue:consume


Mails
-----

Mails are used for several processes, mainly all account related changes (e.g.
registration, chaning password, deletion). The mail server needs to be
configured in your ``data/config.yml`` (see
:doc:`Configuration <configuration>`).


.. _non-php-languages:

Paths to non-php languages
--------------------------

Not all tools that we use are written in php, but the following languages are in
general available on common unix systems. You only need to adjust paths in your
``data/config.yml`` (see :doc:`Configuration <configuration>`) in case of
problems.

Perl
    Perl is required to parse .fit files from garmin devices (and it is highly
    recommended to import fit files instead of tcx or similar).
Python
    Python (v3.x) is required from v4.0 on to generate svg posters.


.. _non-php-tools:

Non-php tools
-------------

Several non-php tools need to be installed locally.

ttbincnv
    Reading ttbin files from TomTom requires an additional tool. A compiled
    version is available at ``call/perl/ttbincnv`` but you may need to compile
    `ryanbinns/ttwach <https://github.com/ryanbinns/ttwatch>`_ specifically for
    your system.
rsvg-convert
    From v4.0 on, our poster tool requires ``rsvg-convert`` to convert svg files
    to png files.
inkscape
    From v4.0 on, our poster tool requires inkscape to convert svg files
    generated as type circular to png files.


Elevation data from srtm files
------------------------------

Elevation data from gps files is not very accurate. We highly recommend to use
satellite data to correct elevation data of your activities. There exist free
APIs but it's much faster to use local files.

Respective data can be downloaded via `dwtkns <http://dwtkns.com/srtm/>`_. You
need to place all relevent \*.tif files for your region into ``data/srtm``.


API Keys
--------

We use several APIs for different purposes. API keys and tokens have to be set
in your ``data/config.yml`` (see :doc:`Configuration <configuration>`).

Weather data
    Loading weather data automatically is possible via
    `openweathermap.org <http://openweathermap.org/api>`_ (free plan offers
    recent data only) and `darksky.net <http://darksky.net/dev>`_.
Map layer
    Additional map layery by Nokia HERE require an API key, see
    `developer.here.com <https://developer.here.com/>`_
Elevation data
    Elevation data can be retrieved via several services. Local srtm files are
    recommended, but geonames: `geonames.org <http://www.geonames.org/>`_
    offers free data as well.


Time zone detection
-------------------

*From v2.5 on*

Most activity files do not correctly specify the time zone. 
Time zone detection can be done based on coordinates but requires a special
database and additional php extensions:

Required packages:

* php-sqlite3
* sqlite3
* libsqlite3-mod-spatialite (at least on ubuntu 16.04)

Database file (has to be stored as ``data/timezone.sqlite``:

* http://cdn.runalyze.com/update/timezone.sqlite

In addition, you need to set ``sqlite3.extension_dir`` in your ``php.ini`` to
wherever ``mod_spatialite`` is located.


Local templates
---------------

*From v3.0 on, see* :doc:`Templates <templates>`

You may overwrite all existing templates (located in ``app/Resources/views``)
with local templates in ``data/views``. Still, it's your responsibility to
adjust them if base templates change in a new version.

Examplary usages:

* analytics.html.twig - for some analytics tool like piwik
* maintenance.html.twig - for maintenance messages