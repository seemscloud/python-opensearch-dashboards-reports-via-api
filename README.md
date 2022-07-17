```python
import sys
import os
import requests
import time

OPD_PROTOCOL = "http"
OPD_ADDRESS = "172.18.0.241"
OPD_PORT = "5601"

OSD_HEADERS = {
    "Accept": "*/*",
    "Connection": "keep-alive",
    "Content-Length": "0",
    "Content-Type": "application/json",
    "osd-version": "2.0.1"
}

OSD_CREDS = ('admin', 'admin')


def current_ms_time():
    return int(time.time() * 1000)


def diff_time(start_time):
    return (current_ms_time() - start_time) / 1000


def post_request(url, headers, creds):
    start_time = current_ms_time()
    try:
        resp = requests.get(url=url, headers=headers, auth=creds, timeout=500)
        print(resp.content)
        print(resp.status_code)
        print(resp.text)
    except:
        pass
    print(diff_time(start_time))


class OpensearchDashboardsAPI:
    def __init__(self):
        self.endpoint = "{}://{}:{}".format(OPD_PROTOCOL, OPD_ADDRESS, OPD_PORT)

    def get_endpoint(self):
        return self.endpoint


class ReportsAPI(OpensearchDashboardsAPI):
    def api_prefix(self):
        return "api/reporting/generateReport"

    def default_params(self):
        return "timezone=Europe%2FWarsaw&dateFormat=MMM%20D%2C%20YYYY%20%40%20HH%3Amm%3Ass.SSS&csvSeparator=%2C"

    def build_uri(self, report_id):
        return "{}/{}/{}?{}".format(self.endpoint, self.api_prefix(), report_id, self.default_params())

    def get_report(self, report_id):
        post_request(self.build_uri(report_id), OSD_HEADERS, OSD_CREDS)


osdApi = ReportsAPI()

osdApi.get_report("ZgpXDYIBQuR7Fc7bTwn8")
```
