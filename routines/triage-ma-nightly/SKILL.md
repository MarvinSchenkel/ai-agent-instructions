---
name: triage-ma-nightly
description: Debug any issues for MA
---

I want you to download the latest 24 hours of Music Assistant logs from my Home Assistant instance that is running the Music Assistant nightly addon. Then:
- Scan the logs for errors
- Scan the logs for event loop blocks that indicate bugs
- Scan the logs for other obvious bugs

Report what you found in an actual report, categorized by severity. Whenever I click an issue, I can see all the details. In the details page, I also want a button 'start chip session to fix this', so I can quickly react to issues that popup.

MA URL: 192.168.1.80:8095
MA token: <YOUR_MA_LONG_LIVED_TOKEN>
Api command: logging/get