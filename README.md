Description:
------------

These scripts are meant to query NOAA data that is in the public domain and is accessible via website.

**send-alert**:

This script will conditionally send email alerts to addresses specified under the alias flare_alert_group.
It can be placed in cron to run every minute. It will not send more than one alert per hour.

It takes no arguments.

**get-xray-flux**:

This script uses wget to fetch and parse current X-Ray flux levels.

It takes no arguments.

It returns the current X-Ray flux (wavelengths: 1e-10m - 8e-10m) in watts / (meter ^ 2), as measured by the GOES-15 satellite.
More details about data source below.

Installation instructions (for Linux):
======================================

With sendmail/postfix installed, add an alias to $HOME/.mailrc (create that file if necessary) called flare_alert_group and add subscribers email addresses, comma separated like so:

> <pre>alias flare_alert_group subscriber1@example.com,subscriber2@example.com</pre>

You can also add cell phone numbers so that subscribers receive text messages:

- Verizon: {10 digit cell number}@vtext.com
- Sprint: {10 digit cell number}@messaging.sprintpcs.com
- AT&T: {10 digit cell number}@txt.att.net

Add send-alert script to $HOME/bin (create directory if necessary)

Run crontab -e and add the following entry:

> <pre># m h dom mon dow command</pre>
> <pre>* * * * * /home/ubuntu/bin/send-alert</pre>

*Note*:
This script will create a small file (a few bytes) in $HOME called .lastAlert which is used to keep a timestamp of when the last alert was sent. It is needed in order to prevent the sending of too many alerts in the duration of an x-class flare event. This file name/location can be changed in the script (in case of namespace collision).

Goals:
------

To build an alert system and use cron to periodically monitor solar activity.

<pre>
========< Sample of data source file >========
:Data_list: Gp_xr_1m.txt

# Prepared by the U.S. Dept. of Commerce, NOAA, Space Weather Prediction Center
# Please send comments and suggestions to SWPC.Webmaster@noaa.gov 
# 
# Label: Short = 0.05- 0.4 nanometer
# Label: Long  = 0.1 - 0.8 nanometer
# Units: Short = Watts per meter squared
# Units: Long  = Watts per meter squared
# Source: GOES-15
# Location: W136
# Missing data: -1.00e+05
#
#         1-minute GOES-15 Solar X-ray Flux
# 
#                 Modified Seconds
# UTC Date  Time   Julian  of the
# YR MO DA  HHMM    Day     Day       Short       Long
#-------------------------------------------------------
2012 03 10  0035   55996   2100     1.50e-07    2.06e-06
2012 03 10  0036   55996   2160     1.49e-07    2.12e-06
2012 03 10  0037   55996   2220     1.37e-07    2.11e-06
</pre>
