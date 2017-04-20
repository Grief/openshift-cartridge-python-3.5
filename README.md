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

### Statirng your python project

##### requirements.txt
```
gunicorn
flask
```

##### app.py
```
from flask import Flask

app = Flask(__name__)
app.config['SECRET_KEY'] = 'changeit'

@app.route('/')
def index():
    return 'Hello World'

```
### Changing the 'start action_hook'

```
cd <app-name>
cd .openshift/action_hooks
```


### Need update your .openshif/action_hooks/start bash file

```
nohup $HOME/python/usr/bin/pip3 install -r $OPENSHIFT_REPO_DIR/requirements.txt
cd $OPENSHIFT_REPO_DIR
nohup $HOME/python/usr/bin/gunicorn app:app --bind=$OPENSHIFT_DIY_IP:8080 |& /usr/bin/logshifter -tag diy &

```





