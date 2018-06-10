If a webpage is formatted by RSS, there are two ways for us to get text data from a webpage. One way is to parse HTML page, the other way is to parse the XML in RSS feed.

At first, I use an external python package called BeautifulSoup to extract data from HTML page, thanks to the reminder from my mentor, I realized that the result is fragile to parse HTML. Therefore, I decide to parse the XML in RSS feed. Also, to parse an XML in RSS feed, it is easier to use the XML parser in the standard python.

In general, there are two reasons for me to parse XML in RSS feed using XML parser:

1. It is more stable to parse XML than to parse HTML. The webpage we are scraping might change frequently, if we extract data from HTML, our code might no longer work once the web template is changed. I use BeautifulSoup before to extract data from HTML. To use BS, I extract data by finding data in each tag visually. For example, I extract a comment within <comment> blahblahblah <,comment>, however, once the web template is changed, this comment "blahblahblah" might be stored in a new tag as <others> blahblahblah </others>, if so, my codes does not work anymore. We do not need to worry about this problem parsing XML in RSS feed since RSS feed is a stable file format.

2. It is more efficient. After deciding to parse XML but not to parse HTML, I need a tool to help me. XML parser in the standard python would be the best choice. It is efficient and user friendly. For example, I need to parse the yellow section of #2988 ticket information on: https://devel.rtems.org/ticket/2988

Code is short:

```python
import codecs
import csv
import rtems_trac
import urllib.request


class TicketParser:
    def __init__(self, number):
        self.ticket_rss = rtems_trac.ticket_rss(number)
        self.ticket_csv = rtems_trac.ticket_csv(number)
        self.ticket_csv = rtems_trac.ticket_csv(number)

    def parse_ticket_meta(self):
        self._parse_ticket_csv()

    def get_ticket_meta(self):
        return self.get_ticket_meta()

    def _parse_ticket_csv(self):
        ticket_csv_url = urllib.request.urlopen(self.ticket_csv)
        csv_table = csv.DictReader(codecs.iterdecode(ticket_csv_url, 'utf-8-sig'))
        print(next(csv_table))

tp = TicketParser(2988)
tp.parse_ticket_meta()
```
```python
trac_base = 'https://devel.rtems.org'
ticket_base = trac_base + '/ticket'
rss = 'format=rss'
csv = 'format=csv'


def ticket_url(number):
    return ticket_base + '/' + str(number)


def ticket_rss(number):
    return ticket_url(number) + '?' + rss


def ticket_csv(number):
    return ticket_url(number) + '?' + csv
```
