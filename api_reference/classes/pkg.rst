.. index:: pkg class
.. _pkg:

pkg
***

The pkg class is used to manage software packages.

Every pkg class request contains several parameters:

+---------------+-----------+-------------------------------------------+
| Parameter     | Value     | Description                               |
|               |           |                                           |
+===============+===========+===========================================+
| id            |           | Any unique value for the request,         |
|               |           | including a hash, checksum, or uuid.      |
+---------------+-----------+-------------------------------------------+
| name          | pkg       |                                           |
|               |           |                                           |
+---------------+-----------+-------------------------------------------+
| namespace     | sysadm    |                                           |
|               |           |                                           |
+---------------+-----------+-------------------------------------------+
| action        |           | Supported actions include                 |
|               |           | "list_categories", "list_repos",          |
|               |           | "pkg_audit", "pkg_autoremove",            |
|               |           | "pkg_check_upgrade", "pkg_info",          |
|               |           | "pkg_install", "pkg_lock", "pkg_remove",  |
|               |           | "pkg_search", "pkg_unlock", "pkg_update", |
|               |           | and "pkg_upgrade"                         |
|               |           |                                           |
+---------------+-----------+-------------------------------------------+

The rest of this section provides examples of the available *actions*
for each type of request, along with their responses.

.. index:: pkg_info, pkg
.. _Package Information:

Package Information
===================

The "pkg_info" action reads the pkg database directly and returns any
relevant information. The following arguments are optional:

* **"repo"**: Unless specified, defaults to the local package repository.

* **"pkg_origins"**: Unless specified, information for all installed
  packages will be listed.

* **"category"**: Limits the results to packages within the specified
  category.

* **"result"**: Must be set to "full" to retrieve all of the information
  with multiple possible values, such as "dependencies", "options", and
  "licences".

**REST Request**

::

 PUT /sysadm/pkg
 {
   "pkg_origins" : [
      "x11/lumina"
   ],
   "repo" : "local",
   "action" : "pkg_info"
 }

**WebSocket Request**

.. code-block:: json

 {
   "name" : "pkg",
   "namespace" : "sysadm",
   "args" : {
      "repo" : "local",
      "action" : "pkg_info",
      "pkg_origins" : [
         "x11/lumina"
      ]
   },
   "id" : "fooid"
 }

**Response**

::

 {
  "args": {
    "pkg_info": {
      "x11/lumina": {
        "arch": "FreeBSD:11:amd64",
        "automatic": "0",
        "comment": "Lumina Desktop Environment",
        "dep_formula": "",
        "dependencies": [
          "x11-toolkits/qt5-gui",
          "x11/qt5-x11extras",
          "x11-wm/fluxbox",
          "x11/libXdamage",
          "devel/qt5-linguist",
          "x11/numlockx"
        ],
        "desc": "Lumina-DE is a lightweight, BSD licensed desktop environment.",
        "files": [
          "/usr/local/share/licenses/lumina-0.8.8_2,1/catalog.mk",
          "/usr/local/share/licenses/lumina-0.8.8_2,1/LICENSE",
          "/usr/local/share/licenses/lumina-0.8.8_2,1/BSD3CLAUSE",
          "/usr/local/bin/Lumina-DE",
          "/usr/local/bin/lumina-config",
          "/usr/local/bin/lumina-fileinfo",
          "/usr/local/bin/lumina-fm",
          "/usr/local/bin/lumina-info",
          "/usr/local/bin/lumina-open",
          "/usr/local/bin/lumina-screenshot",
          "/usr/local/bin/lumina-search",
          "/usr/local/bin/lumina-xconfig",
          "/usr/local/etc/luminaDesktop.conf.dist",
          "/usr/local/include/LuminaOS.h"
        ],
        "flatsize": "12324767",
        "icon": "\\\"http://www.pcbsd.org/appcafe/icons/x11_lumina.png\\\"",
        "id": "2541",
        "licenselogic": "1",
        "licenses": [
          "BSD3CLAUSE"
        ],
        "locked": "0",
        "maintainer": "kmoore@FreeBSD.org",
        "manifestdigest": "2$0$4ypg5zrco9upyuioczmo3uwbtdd5yart7xuit6fx3gjrn1k979qb",
        "message": "The Lumina Desktop Environment has been installed!",
        "mtree_id": "",
        "name": "lumina",
        "options": {
          "MULTIMEDIA": "on",
          "PCBSD": "on"
        },
        "origin": "x11/lumina",
        "pkg_format_version": "",
        "prefix": "/usr/local",
        "repo_type": "binary",
        "repository": "pcbsd-major",
        "screen1": "\\\"http://www.pcbsd.org/appcafe/screenshots/x11/lumina/screen1.png\\\"",
        "shlibs_provided": [
          "libLuminaUtils.so.1"
        ],
        "shlibs_required": [
          "libxcb.so.1",
          "libxcb-composite.so.0",
          "libxcb-damage.so.0",
          "libXdamage.so.1",
          "libxcb-util.so.1",
          "libGL.so.1"
        ],
        "time": "1458334158",
        "version": "0.8.8_2,1",
        "www": "http://lumina-desktop.org"
      }
    }
  },
  "id": "fooid",
  "name": "response",
  "namespace": "sysadm"
 }

.. index:: pkg_search, pkg
.. _Search Packages:

Search Packages
===============

The "pkg_search" action searches the package database for pkgs which
match the given "search_term" (required). These parameters are optional:

* **"repo"**: May be used to specify searching the specified repository.
  If not specified, the local package database is searched.

* **"category"**: May be used to restrict searches to the specified
  package category.

**REST Request**

::

 PUT /sysadm/pkg
 {
   "repo" : "pcbsd-major",
   "category" : "www",
   "action" : "pkg_search",
   "search_term" : "fire",
   "search_excludes" : ["<phrase1>", "<phrase2>"]
 }

**WebSocket Request**

.. code-block:: json

 {
   "id" : "fooid",
   "namespace" : "sysadm",
   "name" : "pkg",
   "args" : {
      "action" : "pkg_search",
      "search_term" : "fire",
      "search_excludes" : ["<phrase1>", "<phrase2>"],
      "category" : "www",
      "repo" : "pcbsd-major"
   }
 }

**Response**

.. code-block:: json

 {
  "args": {
    "pkg_search": {
      "results_order" : ["www/firefox", "www/firefox-esr", "www/firefox-esr-i18n", "www/firefox-pulse"],
      "www/firefox": {
        "arch": "FreeBSD:11:amd64",
        "cksum": "cc72c379afbd66d152cf06b7d2a14ada413f338071ecb9b084899c94d39f951e",
        "comment": "Web browser based on the browser portion of Mozilla",
        "cpe": "cpe:2.3:a:mozilla:firefox:45.0:::::freebsd11:x64:1",
        "dep_formula": "",
        "desc": "Mozilla Firefox is a free and open source web browser descended from the\nMozilla Application Suite. It is small, fast and easy to use, and offers\nmany advanced features:\n\n o Popup Blocking\n o Tabbed Browsing\n o Live Bookmarks (ie. RSS)\n o Extensions\n o Themes\n o FastFind\n o Improved Security\n\nWWW: http://www.mozilla.com/firefox",
        "flatsize": "96435169",
        "icon": "\\\\\\\"http://www.pcbsd.org/appcafe/icons/www_firefox.png\\\\\\\"",
        "id": "12147",
        "licenselogic": "1",
        "maintainer": "gecko@FreeBSD.org",
        "manifestdigest": "2$0$hcbb9x7urbs9nw1e44chw9bwxn339983b6q9mixxdn5ghdwuh9ny",
        "name": "firefox",
        "no_provide_shlib": "yes",
        "olddigest": "",
        "origin": "www/firefox",
        "osversion": "",
        "path": "All/firefox-45.0_1,1.txz",
        "pkg_format_version": "",
        "pkgsize": "39935776",
        "prefix": "/usr/local",
        "screen1": "\\\\\\\"http://www.pcbsd.org/appcafe/screenshots/www/firefox/screen1.png\\\\\\\"",
        "screen2": "\\\\\\\"http://www.pcbsd.org/appcafe/screenshots/www/firefox/screen2.png\\\\\\\"",
        "version": "45.0_1,1",
        "www": "http://www.mozilla.com/firefox"
      },
      "www/firefox-esr": {
        "arch": "FreeBSD:11:amd64",
        "cksum": "811545c4da089b52db54ddee04af2ea8c439eb12e708f478b09141cdcca7aec5",
        "comment": "Web browser based on the browser portion of Mozilla",
        "cpe": "cpe:2.3:a:mozilla:firefox_esr:38.7.0:::::freebsd11:x64",
        "dep_formula": "",
        "desc": "Mozilla Firefox is a free and open source web browser descended from the\nMozilla Application Suite. It is small, fast and easy to use, and offers\nmany advanced features:\n\n o Popup Blocking\n o Tabbed Browsing\n o Live Bookmarks (ie. RSS)\n o Extensions\n o Themes\n o FastFind\n o Improved Security\n\nWWW: http://www.mozilla.com/firefox",
        "flatsize": "86940998",
        "icon": "\\\\\\\"http://www.pcbsd.org/appcafe/icons/www_firefox-esr.png\\\\\\\"",
        "id": "656",
        "licenselogic": "1",
        "maintainer": "gecko@FreeBSD.org",
        "manifestdigest": "2$0$km1kyyxoae47gyhp9gx7wz7pcnsn6jnc8yxgpz63iyynaxi7ia8y",
        "name": "firefox-esr",
        "no_provide_shlib": "yes",
        "olddigest": "",
        "origin": "www/firefox-esr",
        "osversion": "",
        "path": "All/firefox-esr-38.7.0,1.txz",
        "pkg_format_version": "",
        "pkgsize": "36352676",
        "prefix": "/usr/local",
        "version": "38.7.0,1",
        "www": "http://www.mozilla.com/firefox"
      },
      "www/firefox-esr-i18n": {
        "arch": "FreeBSD:11:*",
        "cksum": "c389f2960fa77548435e0b905b3ef6ddb48957b76c2d8346de1f9f97dd7b23ca",
        "comment": "Localized interface for Firefox",
        "dep_formula": "",
        "desc": "Language packs for Firefox\n\nWWW: http://www.mozilla.org/projects/l10n/",
        "flatsize": "102671800",
        "id": "17350",
        "licenselogic": "1",
        "maintainer": "gecko@FreeBSD.org",
        "manifestdigest": "2$0$wzmx16rcynpdej5eckeg6c8w8z6r7oha86cmjfth4pnfu9iojdmb",
        "name": "firefox-esr-i18n",
        "olddigest": "",
        "origin": "www/firefox-esr-i18n",
        "osversion": "",
        "path": "All/firefox-esr-i18n-38.7.0.txz",
        "pkg_format_version": "",
        "pkgsize": "10449532",
        "prefix": "/usr/local",
        "version": "38.7.0",
        "www": "http://www.mozilla.org/projects/l10n/"
      },
      "www/firefox-i18n": {
        "arch": "FreeBSD:11:*",
        "cksum": "11ca74215bb2c9032a316692b02d4b675cc2102b0e6c9c9f79e85cb6a292e689",
        "comment": "Localized interface for Firefox",
        "dep_formula": "",
        "desc": "Language packs for Firefox\n\nWWW: http://www.mozilla.org/projects/l10n/",
        "flatsize": "107852121",
        "id": "11462",
        "licenselogic": "1",
        "maintainer": "gecko@FreeBSD.org",
        "manifestdigest": "2$0$hozjo4sqt3kn4rqak7hfr4zubt3yahigcnhmbwad7xtuqt1qxntb",
        "name": "firefox-i18n",
        "olddigest": "",
        "origin": "www/firefox-i18n",
        "osversion": "",
        "path": "All/firefox-i18n-45.0.txz",
        "pkg_format_version": "",
        "pkgsize": "10295024",
        "prefix": "/usr/local",
        "version": "45.0",
        "www": "http://www.mozilla.org/projects/l10n/"
      },
      "www/firefox-pulse": {
        "arch": "FreeBSD:11:amd64",
        "cksum": "76bcc4096c378a647c4517ab8fac64d3ecbf2c08a1e47ab0eb9061d95d86c195",
        "comment": "Web browser based on the browser portion of Mozilla",
        "cpe": "cpe:2.3:a:mozilla:firefox:45.0:::::freebsd11:x64:1",
        "dep_formula": "",
        "desc": "Mozilla Firefox is a free and open source web browser descended from the\nMozilla Application Suite. It is small, fast and easy to use, and offers\nmany advanced features:\n\n o Popup Blocking\n o Tabbed Browsing\n o Live Bookmarks (ie. RSS)\n o Extensions\n o Themes\n o FastFind\n o Improved Security\n\nWWW: http://www.mozilla.com/firefox",
        "flatsize": "96438909",
        "icon": "\\\\\\\"http://www.pcbsd.org/appcafe/icons/www_firefox-pulse.png\\\\\\\"",
        "id": "5534",
        "licenselogic": "1",
        "maintainer": "gecko@FreeBSD.org",
        "manifestdigest": "2$0$8mb8qqmcqu3ja8uy4x9nqgyeennjemumrb1q6ugyege76i4rdefb",
        "name": "firefox-pulse",
        "no_provide_shlib": "yes",
        "olddigest": "",
        "origin": "www/firefox-pulse",
        "osversion": "",
        "path": "All/firefox-pulse-45.0_1,1.txz",
        "pkg_format_version": "",
        "pkgsize": "39959876",
        "prefix": "/usr/local",
        "screen1": "\\\\\\\"http://www.pcbsd.org/appcafe/screenshots/www/firefox/screen1.png\\\\\\\"",
        "screen2": "\\\\\\\"http://www.pcbsd.org/appcafe/screenshots/www/firefox/screen2.png\\\\\\\"",
        "version": "45.0_1,1",
        "www": "http://www.mozilla.com/firefox"
      }
    }
  },
  "id": "fooid",
  "name": "response",
  "namespace": "sysadm"
 }

.. index:: list_categories, pkg
.. _List Categories:

List Categories
===============

The "list_categories" action lists all the known, non-empty categories
within the specified repository or, if no repository is specified, the
local repository.

**REST Request**

::
 
 PUT /sysadm/pkg
 {
   "repo" : "local",
   "action" : "list_categories"
 }

**WebSocket Request**

.. code-block:: json

 {
   "id" : "fooid",
   "args" : {
      "action" : "list_categories",
      "repo" : "local"
   },
   "namespace" : "sysadm",
   "name" : "pkg"
 }

**Response**

.. code-block:: json

 {
  "args": {
    "list_categories": [
      "ports-mgmt",
      "x11",
      "gnome",
      "textproc",
      "devel",
      "python",
      "misc",
      "print",
      "graphics",
      "security",
      "x11-fonts",
      "lang",
      "ipv6",
      "perl5",
      "converters",
      "math",
      "x11-toolkits",
      "sysutils",
      "dns",
      "net",
      "accessibility",
      "databases",
      "shells",
      "x11-themes",
      "multimedia",
      "audio",
      "www",
      "ftp",
      "net-im",
      "archivers",
      "comms",
      "java",
      "deskutils",
      "kde",
      "mail",
      "editors",
      "emulators",
      "games",
      "irc",
      "japanese",
      "news",
      "x11-servers",
      "tk",
      "net-mgmt",
      "ruby",
      "x11-drivers",
      "x11-wm",
      "x11-clocks",
      "kld",
      "tcl",
      "enlightenment",
      "linux"
    ]
  },
  "id": "fooid",
  "name": "response",
  "namespace": "sysadm"
 }

.. index:: list_repos, pkg
.. _List Repositories:

List Repositories
=================

The "list_repositories" action scans the package repository configuration
files and returns the names of the available repositories. All of the
repositories returned by this action are valid as the optional "repo"
argument for the other pkg API actions.

**REST Request**

::

 PUT /sysadm/pkg
 {
   "action" : "list_repos"
 }

**WebSocket Request**

.. code-block:: json

 {
   "id" : "fooid",
   "namespace" : "sysadm",
   "name" : "pkg",
   "args" : {
      "action" : "list_repos"
   }
 }

**Response**

.. code-block:: json

 {
  "args": {
    "list_repos": [
      "local",
      "pcbsd-major"
    ]
  },
  "id": "fooid",
  "name": "response",
  "namespace": "sysadm"
 }

.. index:: pkg_audit, pkg
.. _Audit Packages:

Audit Packages
==============

The "pkg_audit" action performs an audit of all installed packages and
reports any packages with known vulnerabilities as well as other
packages which are impacted by those vulnerabilities.

.. note:: The vulnerability information will be returned as a dispatcher
   event as this action just queues up the results of the :command:`pkg`
   operation. This is due to a limitation of :command:`pkg`, as it only
   supports one process call at a time. Refer to the
   :ref:`Dispatcher Subsystem` for instructions on how to subscribe to
   and query dispatcher events.

**REST Request**

::

 PUT /sysadm/pkg
 {
   "action" : "pkg_audit"
 }

**WebSocket Request**

.. code-block:: json

 {
   "args" : {
      "action" : "pkg_audit"
   },
   "name" : "pkg",
   "id" : "fooid",
   "namespace" : "sysadm"
 }

**Response**

.. code-block:: json

 {
  "args": {
    "pkg_audit": {
      "proc_cmd": "pkg audit -qr",
      "proc_id": "sysadm_pkg_audit-{257cc46b-9178-4990-810a-12416ddfad79}",
      "status": "pending"
    }
  },
  "id": "fooid",
  "name": "response",
  "namespace": "sysadm"
 }

**Dispatcher Events System Reply**

.. code-block:: json

 {
  "namespace" : "events",
  "name" : "dispatcher",
  "id" : "none",
  "args" : {
    "event_system" : "sysadm/pkg",
    "state" : "finished",
    "pkg_log" : "<process log>",
    "action" : "pkg_audit",
    "process_details" : {
      "time_finished" : "<ISO 8601 time date string>",
      "cmd_list" : ["<command 1>", "<command 2>"],
      "return_codes/<command 1>" : "<code 1>",
      "return_codes/<command 2>" : "<code 2>",
      "process_id" : "<random>",
      "state" : "finished"
      }
    }
 }

.. index:: pkg_upgrade, pkg
.. _Upgrade Packages:

Upgrade Packages
================

The "pkg_upgrade" action upgrades all currently installed packages. The
messages from the upgrade will be returned as a dispatcher event. Refer
to the :ref:`Dispatcher Subsystem` for instructions on how to subscribe
to and query dispatcher events.

**REST Request**

::

 PUT /sysadm/pkg
 {
   "action" : "pkg_upgrade"
 }

**WebSocket Request**

.. code-block:: json

 {
   "args" : {
      "action" : "pkg_upgrade"
   },
   "name" : "pkg",
   "namespace" : "sysadm",
   "id" : "fooid"
 }

**Response**

.. code-block:: json

 {
  "args": {
    "pkg_upgrade": {
      "proc_cmd": "pkg upgrade -y",
      "proc_id": "sysadm_pkg_upgrade-{19ace7c9-0d83-4a0d-9249-0b56cb105762}",
      "status": "pending"
    }
  },
  "id": "fooid",
  "name": "response",
  "namespace": "sysadm"
 }

**Dispatcher Events System Reply**

.. code-block:: json

 {
  "namespace" : "events",
  "name" : "dispatcher",
  "id" : "none",
  "args" : {
    "event_system" : "sysadm/pkg",
    "state" : "finished",
    "pkg_log" : "<process log>",
    "action" : "pkg_upgrade",
    "process_details" : {
      "time_finished" : "<ISO 8601 time date string>",
      "cmd_list" : ["<command 1>", "<command 2>"],
      "return_codes/<command 1>" : "<code 1>",
      "return_codes/<command 2>" : "<code 2>",
      "process_id" : "<random>",
      "state" : "finished"
      }
    }
 }  

.. index:: pkg_check_upgrade, pkg
.. _Check Packages:

Check Packages
==============

The "pkg_check_upgrade" action checks to see if there are any package
updates available and returns that information as a dispatcher event.
Refer to the :ref:`Dispatcher Subsystem` for instructions on how to
subscribe to and query dispatcher events.

**REST Request**

::

 PUT /sysadm/pkg
 {
   "action" : "pkg_check_upgrade"
 }

**WebSocket Request**

.. code-block:: json

 {
   "args" : {
      "action" : "pkg_check_upgrade"
   },
   "namespace" : "sysadm",
   "name" : "pkg",
   "id" : "fooid"
 }

**Response**

.. code-block:: json

 {
  "args": {
    "pkg_check_upgrade": {
      "proc_cmd": "pkg upgrade -n",
      "proc_id": "sysadm_pkg_check_upgrade-{c5e9d9a1-7c49-4a70-9d7c-4a84277c83b0}",
      "status": "pending"
    }
  },
  "id": "fooid",
  "name": "response",
  "namespace": "sysadm"
 }

**Dispatcher Events System Reply**

.. code-block:: json

 {
  "namespace" : "events",
  "name" : "dispatcher",
  "id" : "none",
  "args" : {
    "event_system" : "sysadm/pkg",
    "state" : "finished",
    "pkg_log" : "<process log>",
    "action" : "pkg_check_upgrade",
    "updates_available" : "true/false",
    "process_details" : {
      "time_finished" : "<ISO 8601 time date string>",
      "cmd_list" : ["<command 1>", "<command 2>"],
      "return_codes/<command 1>" : "<code 1>",
      "return_codes/<command 2>" : "<code 2>",
      "process_id" : "<random>",
      "state" : "finished"
      }
    }
 }

.. index:: pkg_update, pkg
.. _Update Package Database:

Update Package Database
=======================

The "pkg_update" action instructs :command:`pkg` to update its databases.
This action is typically not required.  It returns any information as a
dispatcher event. Refer to the :ref:`Dispatcher Subsystem` for
instructions on how to subscribe to and query dispatcher events.

If you include "force" = "true", it forces :command:`pkg` to completely
resync all of its databases with all known repositories which may take
some time.

**REST Request**

::

 PUT /sysadm/pkg
 {
   "force" : "true",
   "action" : "pkg_update"
 }

**WebSocket Request**

.. code-block:: json

 {
   "id" : "fooid",
   "name" : "pkg",
   "namespace" : "sysadm",
   "args" : {
      "force" : "true",
      "action" : "pkg_update"
   }
 }

**Response**

.. code-block:: json

 {
  "args": {
    "pkg_update": {
      "proc_cmd": "pkg update -f",
      "proc_id": "sysadm_pkg_update-{8d65bbc5-fefc-4f34-8743-167e61a54c4c}",
      "status": "pending"
    }
  },
  "id": "fooid",
  "name": "response",
  "namespace": "sysadm"
 }

**Dispatcher Events System Reply**

.. code-block:: json

 {
  "namespace" : "events",
  "name" : "dispatcher",
  "id" : "none",
  "args" : {
    "event_system" : "sysadm/pkg",
    "state" : "finished",
    "pkg_log" : "<process log>",
    "action" : "pkg_update",
    "process_details" : {
      "time_finished" : "<ISO 8601 time date string>",
      "cmd_list" : ["<command 1>", "<command 2>"],
      "return_codes/<command 1>" : "<code 1>",
      "return_codes/<command 2>" : "<code 2>",
      "process_id" : "<random>",
      "state" : "finished"
      }
    }
 }

.. index:: pkg_lock, pkg_unlock, pkg
.. _LockUnlock Packages:

Lock/Unlock Packages
====================

The "pkg_lock" action locks the specified "pkg_origins" so that it will
be skipped during a package upgrade and remain at its current version.
When using "pkg_origins", specify either a single package origin string
or an array of package origins.

The "pkg_unlock" action unlocks the previously locked "pkg_origins" so
that it is no longer skipped during a package upgrade.

Both actions return any information as a dispatcher event. Refer to the
:ref:`Dispatcher Subsystem` for instructions on how to subscribe to and
query dispatcher events.

**REST Request**

::

 PUT /sysadm/pkg
 {
   "pkg_origins" : [
      "misc/pcbsd-base"
   ],
   "action" : "pkg_lock"
 }

**WebSocket Request**

.. code-block:: json

 {
   "namespace" : "sysadm",
   "id" : "fooid",
   "name" : "pkg",
   "args" : {
      "pkg_origins" : [
         "misc/pcbsd-base"
      ],
      "action" : "pkg_lock"
   }
 }

**Response**

.. code-block:: json

 {
  "args": {
    "pkg_lock": {
      "proc_cmd": "pkg lock -y misc/pcbsd-base",
      "proc_id": "sysadm_pkg_lock-{352f7f66-d036-4c16-8978-67950957bf22}",
      "status": "pending"
    }
  },
  "id": "fooid",
  "name": "response",
  "namespace": "sysadm"
 }

**Dispatcher Events System Reply**

.. code-block:: json

 {
  "namespace" : "events",
  "name" : "dispatcher",
  "id" : "none",
  "args" : {
    "event_system" : "sysadm/pkg",
    "state" : "finished",
    "pkg_log" : "<process log>",
    "action" : "pkg_lock",
    "process_details" : {
      "time_finished" : "<ISO 8601 time date string>",
      "cmd_list" : ["<command 1>", "<command 2>"],
      "return_codes/<command 1>" : "<code 1>",
      "return_codes/<command 2>" : "<code 2>",
      "process_id" : "<random>",
      "state" : "finished"
      }
    }
 }

**REST Request**

::

 PUT /sysadm/pkg
 {
   "action" : "pkg_unlock",
   "pkg_origins" : "misc/pcbsd-base"
 }

**WebSocket Request**

.. code-block:: json

 {
   "id" : "fooid",
   "args" : {
      "action" : "pkg_unlock",
      "pkg_origins" : "misc/pcbsd-base"
   },
   "name" : "pkg",
   "namespace" : "sysadm"
 }

**Response**

.. code-block:: json

 {
  "args": {
    "pkg_unlock": {
      "proc_cmd": "pkg unlock -y misc/pcbsd-base",
      "proc_id": "sysadm_pkg_unlock-{d1771b41-c1ca-480a-a3ce-42d4eddbfae8}",
      "status": "pending"
    }
  },
  "id": "fooid",
  "name": "response",
  "namespace": "sysadm"
 }

**Dispatcher Events System Reply**

.. code-block:: json

 {
  "namespace" : "events",
  "name" : "dispatcher",
  "id" : "none",
  "args" : {
    "event_system" : "sysadm/pkg",
    "state" : "finished",
    "pkg_log" : "<process log>",
    "action" : "pkg_unlock",
    "process_details" : {
      "time_finished" : "<ISO 8601 time date string>",
      "cmd_list" : ["<command 1>", "<command 2>"],
      "return_codes/<command 1>" : "<code 1>",
      "return_codes/<command 2>" : "<code 2>",
      "process_id" : "<random>",
      "state" : "finished"
      }
    }
 }

.. index:: pkg_install, pkg
.. _Install Packages:

Install Packages
================

The "pkg_install" action installs the specified "pkg_origins" on the
system. When using "pkg_origins", specify either a single package origin
string or an array of package origins. Unless the "repo" is specified,
:command:`pkg` will automatically determine the repository. The install
messages will be returned as a dispatcher event. Refer to the
:ref:`Dispatcher Subsystem` for instructions on how to subscribe to and
query dispatcher events.

**REST Request**

::

 PUT /sysadm/pkg
 {
   "pkg_origins" : "games/angband",
   "action" : "pkg_install",
   "repo" : "pcbsd-major"
 }

**WebSocket Request**

.. code-block:: json

 {
   "name" : "pkg",
   "namespace" : "sysadm",
   "id" : "fooid",
   "args" : {
      "action" : "pkg_install",
      "pkg_origins" : "games/angband",
      "repo" : "pcbsd-major"
   }
 }

**Response**

.. code-block:: json

 {
  "args": {
    "pkg_install": {
      "proc_cmd": "pkg install -y --repository \"pcbsd-major\" games/angband",
      "proc_id": "sysadm_pkg_install-{ae444472-47df-4a65-91eb-013cc82ce4ad}",
      "status": "pending"
    }
  },
  "id": "fooid",
  "name": "response",
  "namespace": "sysadm"
 }

**Dispatcher Events System Reply**

.. code-block:: json

 {
  "namespace" : "events",
  "name" : "dispatcher",
  "id" : "none",
  "args" : {
    "event_system" : "sysadm/pkg",
    "state" : "finished",
    "pkg_log" : "<process log>",
    "action" : "pkg_install",
    "process_details" : {
      "time_finished" : "<ISO 8601 time date string>",
      "cmd_list" : ["<command 1>", "<command 2>"],
      "return_codes/<command 1>" : "<code 1>",
      "return_codes/<command 2>" : "<code 2>",
      "process_id" : "<random>",
      "state" : "finished"
      }
    }
 }

.. index:: pkg_remove, pkg
.. _Uninstall Packages:

Uninstall Packages
==================

The "pkg_remove" action uninstalls the specified "pkg_origins" from the
system. When using "pkg_origins", specify either a single package origin
string or an array of package origins.

The optional "recursive" argument can be set to "true" or "false". The
default is "true", which means that other packages which depend on this
package will also be removed so that there are no broken dependencies.

The uninstall messages will be returned as a dispatcher event. Refer to
the :ref:`Dispatcher Subsystem` for instructions on how to subscribe to
and query dispatcher events.

**REST Request**

::

 PUT /sysadm/pkg
 {
   "recursive" : "false",
   "action" : "pkg_remove",
   "pkg_origins" : "games/angband"
 }

**WebSocket Request**

.. code-block:: json

 {
   "id" : "fooid",
   "name" : "pkg",
   "namespace" : "sysadm",
   "args" : {
      "action" : "pkg_remove",
      "recursive" : "false",
      "pkg_origins" : "games/angband"
   }
 }

**Response**

.. code-block:: json

 {
  "args": {
    "pkg_remove": {
      "proc_cmd": "pkg delete -y games/angband",
      "proc_id": "sysadm_pkg_remove-{2aa844aa-f6a8-4e8f-ae71-b56af735ccb8}",
      "status": "pending"
    }
  },
  "id": "fooid",
  "name": "response",
  "namespace": "sysadm"
 }

**Dispatcher Events System Reply**

.. code-block:: json

 {
  "namespace" : "events",
  "name" : "dispatcher",
  "id" : "none",
  "args" : {
    "event_system" : "sysadm/pkg",
    "state" : "finished",
    "pkg_log" : "<process log>",
    "action" : "pkg_remove",
    "process_details" : {
      "time_finished" : "<ISO 8601 time date string>",
      "cmd_list" : ["<command 1>", "<command 2>"],
      "return_codes/<command 1>" : "<code 1>",
      "return_codes/<command 2>" : "<code 2>",
      "process_id" : "<random>",
      "state" : "finished"
      }
    }
 }

.. index:: pkg_autoremove, pkg
.. _Prune Packages:

Prune Packages
==============

The "pkg_autoremove" action prunes all orphaned packages on the system.

**REST Request**

::

 PUT /sysadm/pkg
 {
  "action" : "pkg_autoremove"
 }

**WebSocket Request**

.. code-block:: json

 {
  "args" : {
     "action" : "pkg_autoremove"
  },
  "name" : "pkg",
  "namespace" : "sysadm",
  "id" : "fooid"
 }

**Response**

.. code-block:: json

 {
 "args": {
   "pkg_autoremove": {
     "proc_cmd": "pkg autoremove -y",
     "proc_id": "sysadm_pkg_autoremove-{19ace7c9-0d83-4a0d-9249-0b56cb105762}",
     "status": "pending"
   }
 },
 "id": "fooid",
 "name": "response",
 "namespace": "sysadm"
 }