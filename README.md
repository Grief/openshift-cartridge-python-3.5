Use this URL to install this cartridge:

    https://raw.githubusercontent.com/Grief/openshift-cartridge-python-3.5/master/metadata/manifest.yml


## New Apps

```
rhc app create <app-name> https://raw.githubusercontent.com/Grief/openshift-cartridge-python-3.5/master/metadata/manifest.yml diy-0.1
```

## Example
### If you need to clone your repo...
```
rhc git-clone <app-name>
```

### Starting your python project using gunicorn + flask
This is an example how you can config openshift for run an python web project (flask in this case) with python3.5
If you know how the WSGI works you can port the configs for other framework like django, uwsgi, etc.
[Link django docs with gunicorn](https://docs.djangoproject.com/en/1.11/howto/deployment/wsgi/gunicorn/)

##### requirements.txt
```
gunicorn
flask
```

##### app.py
```
import sys
from flask import Flask

app = Flask(__name__)
app.config['SECRET_KEY'] = 'changeit'

@app.route('/')
def index():
    return sys.version

```
### Changing the 'start action_hook'

```
cd <app-name>
cd .openshift/action_hooks
```

### Need update your .openshif/action_hooks/start bash file

```
#!/bin/bash
nohup $HOME/python/usr/bin/pip3 install -r $OPENSHIFT_REPO_DIR/requirements.txt
cd $OPENSHIFT_REPO_DIR
nohup $HOME/python/usr/bin/gunicorn app:app --bind=$OPENSHIFT_DIY_IP:8080 |& /usr/bin/logshifter -tag diy &
```


### Need update your .openshift/action_hooks/stop bash file

```
#!/bin/bash
source $OPENSHIFT_CARTRIDGE_SDK_BASH

# The logic to stop your application should be put in this script.
if [ -z "$(ps -ef | grep gunicorn | grep -v grep)" ]
then
    client_result "Application is already stopped"
else
    kill `ps -ef | grep gunicorn | grep -v grep | awk '{ print $2 }'` > /dev/null 2>&1
fi
```

# Remembering this an example with flask and gunicorn, some updates are needed if you want using other libs/frameworks

