# Twistlock Splunk App

_Updated for Twistlock/Prisma Cloud Compute versions 19.11+_

The Twistlock Splunk App allows high priority security incidents from Twistlock to be sampled by Splunk on a user-defined interval and provides in-depth forensic data for incident analysis and response.

The Twistlock Splunk App adds two main components to your Splunk deployment: scripted data inputs that make use of your Twistlock Console’s API to pull incidents and forensics and a sample Splunk dashboard that presents that data.

## Installation

1. Add Twistlock Console credentials and URL (without trailing `/`) to `twistlock/bin/meta/config.json`. For example, your file could look like this:
```json
{
  "credentials": {
    "username": "user",
    "password": "pass"
  },
  "setup": {
    "console_url": "https://your.twistlock.console.url:8083"
  }
}
```
2. Drop the twistlock directory into `$SPLUNK_HOME/etc/apps` on the necessary Splunk host(s). Be sure the directory and its contents are owned by the `splunk` user. See Splunk documentation for specific details based on deployment architecture.

3. Run `$SPLUNK_HOME/bin/splunk restart` to load the app.

4. Enable `poll-incidents.py` and `poll-forensics.py` at **Settings > Data inputs > Scripts**.

5. **Optional:** Adjust the schedule as needed. The `poll-forensics.py` script uses a file created by `poll-incidents.py` to only pull relevant forensics information. Be sure to schedule `poll-forensics.py` to run slightly after `poll-incidents.py`. A few minutes is probably fine. A one-minute gap was used during testing.

## Change notes
#### March 23, 2020
- Combined Windows and Linux `inputs.conf` files. See that file for details.
- Timestamps used in the forensic data now include nanosecond precision.
- Switched from basic authentication to token-based authentication on all API calls.
- Added an `incidentID` field to each forensic event to simplify matching forensic events to their incidents.