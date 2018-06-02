# simple-python-pyinstaller-app

To automatically trigger a build:
# CRUMB=$(wget -q --auth-no-challenge --user ianh --password symmwd --output-document - 'http://localhost:8080/crumbIssuer/api/xml?xpath=concat(//crumbRequestField,":",//crumb)')
# curl -X POST --user ianh:mypassword --data-urlencode json='{"parameter": [{"name":"id", "value":"token2920"}, {"name":"verbosity", "value":"high"}]}' -H "$CRUMB"  http://localhost:8080/job/simple-python-pyinstaller-app-multibranch/job/development/build
# curl -X POST --user ianh:301b876364869898e492eb06b9835fd7 -H "$CRUMB"  http://localhost:8080/job/simple-python-pyinstaller-app-multibranch/job/development/build
