# Writing Content

## Pre-Requisites 
Before you begin, follow the steps to install **dependencies and pre-commit hooks** 
1. Create virtualenv and install requirements: `virtualenv venv && source venv/bin/activate && pip install -r requirements.txt`.
2. Install `pre-commit install`.

## Writing Content
1. Select the content [piece](https://github.com/splunk/security-content/wiki/Content-Structure) you want to write. 
2. Copy an example and edit it to suit your needs. At a minimum, you must write [a detection search](detections/).
3. Make a pull request. The pull request will trigger CircleCI, a continuous-integration app that integrates with a VCS and automatically runs a series of steps every time that it detects a change to your repository. A CircleCI build consists of a series of steps, usually Dependencies, Testing, and Deployment. If your tests pass, you're good to go! If the CircleCI check fails, refer to [troubleshooting](https://github.com/splunk/security-content/wiki/Troubleshooting).  

For a more detailed explanation on how to contribute to the project, please see ["Contributing"](https://github.com/splunk/security-content/wiki/Contributing-to-the-Project)