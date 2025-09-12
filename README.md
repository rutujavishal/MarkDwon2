# Reporting For Tools Test

This script generates sub report for tools as weShare page. 

## Installation 

Use pipx to install the tools test report tool:

```
pipx install git+ssh://git@socgit.advantest.com:7999/test_tool/tools-test-report.git
```

Make sure `$HOME/.local/bin` is in your `$PATH`. If you don't have pipx yet,
install it using `/opt/rh/rh-python38/root/usr/bin/python3.8 -m pip install --upgrade --user pipx`. **Do not** install the tools test report tool using pip directly.

Use `pipx upgrade test-report` to upgrade to the latest version. 

## Usage

```
test-report --environment=prod
```

Or run it with -h to see all options.

## Development

Clone the repository and install the needed libraries with Poetry (>=Python3.8)


```
git clone ssh://git@socgit.advantest.com:7999/test_tool/tools-test-report.git
cd tools-test-report
# add virtual environment and activate
python -m venv .venv
source .venv/bin/activate
pip install -r requirements_global.txt
pip install -r requirements.txt
```

Compile an executable with Pex (will be stored in the project folder)
```
./build-pex
```
## Environment Variables

More configurations that can be done in the ".env_xxx" file ("xxx" is either "prod","test","dev" or "cm"):
 * PAGE_PREFIX = "Oli "
   * add this prefix to the title of the generated report page (if multiple developers work in parallel on tasks and have to generate a report for their own)
   * if this variable if missing, no prefix is used (default)
 * ErrorEmailNotification="1"
   * enable the mail notification (see next points)
 * SENDER_EMAIL = "oliver.schuetze@advantest.com"
   * this mail address will be used as "sender" for all notification mails sent during the report generation
 * DEVELOPER_EMAILS = '["ramesh.reddy@ptn.advantest.com","oliver.schuetze@advantest.com"]'
   * list of mail addresses (of developers) who will be informed by mail in case of a script error during the report generation
 * level="info"
   * level of log messages
 * logToFile="0"
   * store log files
 * weShareSpace="SIEMADV"
   * used weShare space for report
 * pageID="269298894"
   * used parent page for report
 * test="1"
   * if 1, then use weShareTEST and jira-TEST instead of weShare and weTrack
 * release="current"
   * if "current", all currently active STCs are processed - otherwise define a single release, e.g. "8.6.3.0"
 * BA_DB_NAME="buildanalyzer"
 * BA_DB_USERNAME="..."
 * BA_DB_PASS="..."
 * BA_DB_PORT="..."
 * BA_DB_HOST="..."
 * DEPLOYMENT_BUILD_ID="0"
   * updated by CI/CD pipeline - can be used to print the build ID in the weShare report
 * DEPLOYMENT_IMAGE_TAG="00-xxx"
   * updated by CI/CD pipeline - can be used to print the image tag in the weShare report

The authentification can be done in different variants:
 * USERNAME="..."
 * if you use API access token (this can be generated in the personal settings of weShare or weTrack):
   * WESHARE_TOKEN="..."
   * WETRACK_TOKEN="..."
   * (these token are not added to the secret until JIRA-10166 is resolved)
 * if  you use a pre-build token for basic authentification
   * WESHARE_AUTHENTIFICATION_TOKEN="..."
   * WETRACK_AUTHENTIFICATION_TOKEN="..."
   * the token can be generated using the shell command echo -n 'your_username:your_password' | base64
 * if you use username and tool-specific password
   * WESHARE_PASSWORD="..."
   * WETRACK_PASSWORD="..."
 * if you use username and password that fits to weShare and weTrack
   * PASSWORD="..."
