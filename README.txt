//$Id$

NCSU Roles module v6.x-1.1
==========================
By brabec at ncsu.edu

DESCRIPTION
-----------
This module automatically populates Drupal User Roles using various 
NCSU group sources. 

PREREQUISITES
-------------
You should be running wraplogin for your primary user authentication. 
This module assumes that WRAP users will be named in the database as 
"unityid.ncsu.edu", which is the standard used by the wraplogin module. 
All of the group memberships that are used are based on Unity accounts.

INSTALLATION
------------
Place the entire ncsuroles module folder into your third-party modules
directory, typically at sites/all/modules.

Enable the module in Drupal by going to 
    Administer >> Site building >> Modules.

Set up ncsuroles.module by going to 
    Administer >> User management >> NCSU Roles.

CONFIGURATION
-------------
Create a new role using Administer >> User management >> Roles
    - select the name for your new role and click "Add Role"

Once created, click on the "edit role" link next to the new role.

On the edit form:
    - check the Enable box
    - select the Source Type (see below)
    - enter the Group Key (see below) for the source group
    - click "Save role"

If your key checks out, the role will be added to the database. 
The next time the cron update runs, your role will be populated
with any of the member users who already have Drupal login accounts.

If a user signs in to WRAP at a later date, the module will add their
roles at that time, rather than waiting for the next cron run.

GROUP TYPES
-----------

AFS PTS Groups
    - this type can include any group defined in our PTS
    - the default AFS cell is "unity"
    - the format of the key is "owner:group@cell"
    - for example, group "cccadm" in the default cell is "cccadm@unity"
    - a personal group named "test" owned by me would be "brabec:test@unity"

Hesiod Groups
    - this type can include any of the groups reported by the 'hes'
      command on a unix machine
    - the group key is just the hesiod group name
    - for example, "hes brabec grplist" returns:
        ncsu_staff:324:ncsu:108:cccadm:110:cc-staff:320
      so we could use "ncsu_staff", "ncsu", "cccadm", or "cc-staff"

SysNews Groups
    - this type can include any of the SysNews SysTools permissions groups
    - the group key is the "groupid" in SysTools, which is usually
      a short acronym ID.
    - all of the OIT organizational groups are available from SysNews:
      e.g. "oit", "oit-iso", "oit-iso-shs", etc.

UPDATES
-------
The cron update runs every time that cron.php is called for your installation.
However, this module has a built-in refresh period that prevents cron from 
polling all the groups too often. The default polling period is every 
24 hours, but that can be reduced to every 4 or 8 hours on the 
module configuration page.
