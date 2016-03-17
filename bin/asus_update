#!/usr/bin/python3
# -*- coding: utf-8 -*-
import urllib.request, lxml.html, re
from collections import OrderedDict

DEVICES=OrderedDict([
    ('ZE500KL', 'https://www.asus.com/support/Download/39/1/0/20/KsY29RCpsJ2bea9O/32/'),
    ('ZE500KG', 'https://www.asus.com/support/Download/39/1/0/21/xY9Hj3GM8ZlMxOR8/32/'),
    ('ZE550KL', 'https://www.asus.com/support/Download/39/1/0/22/ZROQURlyupei1ZQz/32/'),
    ('ZE551KL', 'https://www.asus.com/support/Download/39/1/0/23/SZCx58yhB5Jst0yI/8/'),
    ('ZD551KL', 'https://www.asus.com/support/Download/39/1/0/17/a3iCfYUnl9DJAg41/32/'),
    ('ZE600KL', 'https://www.asus.com/support/Download/39/1/0/24/WnVZyhpDzHw3JqH0/32/'),
    ('ZE601KL', 'https://www.asus.com/support/Download/39/1/0/25/X3KCz1JmjdVkQtTo/32/'),
])
OS_XPATH = b'//*[@id="div_type_20"]//a'
VERSION_REGEX = '(WW|CN|JP|TW|RKT)|(\d\.\d+.\d+\.\d+)'

for device in DEVICES:
    print("%s:" % device)

    url = DEVICES[device]
    req = urllib.request.Request(
        url,
        data=None,
        headers={
            'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/35.0.1916.47 Safari/537.36'
        }
    )
    data = urllib.request.urlopen(req).read()
    html = lxml.html.fromstring(data.decode('utf-8'))

    for os in html.xpath(OS_XPATH):
        href = os.get("href")

        if href != None:
            region = re.findall(VERSION_REGEX, href)[0][0]
            version = re.findall(VERSION_REGEX, href)[1][1]

            print("V%s (%s) : %s" % (version, region, href))
