purpose
-------

anacron helper that wakes up desktop pc to run using the RTC, then optionally puts it back to sleep afterwards (if the PC has not been disturbed).

optionally supports running arbitary processes on suspend + resume (user initiated and/or automatic wakeup).

requirements
------------

- rtc wake on timer enabled in bios/uefi config
- anacron already setup
- rtcwake utility (from util-linux on debian)
- [arrow](http://crsmithdev.com/arrow/) (from python-arrow on debian)

setup
-----

first edit rtccron script to run the scripts required, then:

    cp rtccron rtccron_autowake /usr/local/sbin
    cp *.service /etc/systemd/system/
    cd /etc/systemd/system
    systemctl enable rtccron*.service
    systemctl start rtccron-startup
    systemctl mask anacron-resume.service
    
edit /etc/anacrontab  - must contain:
    
    START_HOURS_RANGE=2-4
    
edit /etc/cron.d/anacron -- must run every hour >= 30 minutes past, e.g.:

     30 *    * * *   root        test -x /etc/init.d/anacron && /usr/sbin/invoke-rc.d anacron start >/dev/null

