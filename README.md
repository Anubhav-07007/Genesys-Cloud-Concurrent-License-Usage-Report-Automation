# Genesys Cloud Concurrent License Usage Report Automation

Find out **who** was logged in, **when** concurrent license usage peaked, and **who** stays logged in too long, then deliver the answer as a monthly Excel report.

## Summary

Concurrent licenses in Genesys Cloud CX are easy to overuse and hard to trace. When monthly usage goes above the licensed count, the platform shows the total but not the people and times behind it.

This project validates the required Genesys Cloud APIs in Postman, then uses a Python script to collect the data and export it to Excel. The report shows:

- the **daily peak trend** of concurrent usage,
- the **users who log in most often at peak times**,
- the **login hours per user per day**, with anyone logged in for **12+ hours** highlighted so they can be asked to log out and free the license.

A Windows Task Scheduler job runs the script every month, so a fresh report is ready for review without manual work. The report supports decisions to reduce unused licenses and lower billing cost.

> The Python script was built with AI-assisted tools. Every part was reviewed and tested before use.

## Tech stack

| Area | Tools |
|---|---|
| Platform | Genesys Cloud CX |
| API testing | Postman |
| Automation | Python (requests, pandas, openpyxl, python-dotenv) |
| Scheduling | Windows Task Scheduler (batch file) |
| Output | Excel (.xlsx) |

## How it works

1. **Identify the data needed.** Decide which information answers the questions: user details, login and logout times, and concurrent usage over time.
2. **Validate in Postman.** Test the available Genesys Cloud APIs ([add API names]) and confirm each returns the data needed.
3. **Authenticate.** Use an OAuth client with Client Credentials. The client ID and secret are kept in a local `.env` file, never in the code.
4. **Collect the data.** The Python script requests the data, handles pagination, and joins results with user details.
5. **Calculate.**
   - Build a daily trend of concurrent usage and mark the peak.
   - List the users present most often at peak times.
   - Add up each user's login hours per day and flag anyone above 12 hours.
6. **Export to Excel.** Write a timestamped workbook with separate sheets: daily peak trend, peak-time users, and per-user daily login hours.
7. **Review.** Open the workbook, check the daily trend and the flagged users, and share the findings with the manager.
8. **Schedule.** A batch file runs the script, and Windows Task Scheduler triggers it monthly.

## Report contents

| Sheet | What it shows |
|---|---|
| Daily Peak Trend | Peak concurrent usage per day, to find the busiest days |
| Peak-Time Users | Users who are logged in most often during peak periods |
| User Login Hours | Login hours per user per day, with 12+ hour sessions highlighted |

## Setup

1. Install Python 3 and the libraries:
   ```
   pip install requests pandas openpyxl python-dotenv
   ```
2. Create an OAuth client in Genesys Cloud with the minimum permissions needed to read the data.
3. Create a `.env` file in the project folder:
   ```
   CLIENT_ID=your_client_id
   CLIENT_SECRET=your_client_secret
   REGION=your_genesys_region
   ```
4. Run the script:
   ```
   python license_usage_report.py
   ```
   *(Use your own script name.)*
5. To automate it, create a Windows Task Scheduler task that runs the batch file monthly.

## Security notes

- Credentials live only in `.env`. Add `.env` to `.gitignore`.
- The OAuth client has read-only access to the data it needs.
- No generated reports or user data should be committed to this repository, because the Excel files contain employee names and login details.
- Licensed counts, billing figures and company details are intentionally left out.

## Outcome

A repeatable monthly report that shows license pressure by day and by user, so the team can ask long-running sessions to log out and reduce unused licenses and billing cost.

## Author

**Anubhav Kumar Rajput**: Genesys Cloud CX L3 Support & Automation Engineer
GitHub: [Anubhav-07007](https://github.com/Anubhav-07007)
