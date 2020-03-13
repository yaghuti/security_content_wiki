# Writing Content
Before you begin, follow the steps to install **dependencies and pre-commit hooks** under ["Developing"](https://github.com/splunk/security-content#developing). 

1. Select the content [piece](https://github.com/splunk/security-content#content-parts) you want to write. 
2. Copy an example and edit it to suit your needs. At a minimum, you must write a [story](stories/), [a detection search](detections/), and an [investigative search](investigations/).
3. Make a pull request. The pull request will trigger CircleCI, a continuous-integration app thatintegrates with a VCS and automatically runs a series of steps every time that it detects a change to your repository. A CircleCI build consists of a series of steps, usually Dependencies, Testing, and Deployment. If your tests pass, you're good to go! If the CircleCI check fails, refer to [troubleshooting](https://github.com/splunk/security-content#troubleshooting).  

For a more detailed explanation on how to contribute to the project, please see ["Contributing"](#Contributing)

# Developing
##### Dependecies and Pre-Commit Hooks
Install project dependecies and tests that run before content is committed:

1. Create virtualenv and install requirements: `virtualenv venv && source venv/bin/activate && pip install -r requirements.txt`.
2. Install `pre-commit install`.

##### CI Tools
Tools that help with testing CI jobs:

1. Install CircleCI [CLI Tool](https://circleci.com/docs/2.0/local-cli/).
2. To test a local change to CircleCI or build, make sure you are running Docker, then enter
`circleci local execute -e GITHUB_TOKEN=$GITHUB_TOKEN --branch <your branch>`.

##### Generate Docs from Schema 
To automatically generate docs from schema:

1. Install https://github.com/adobe/jsonschema2md.
2. Enter `jsonschema2md -d spec/v2/detections.spec.json -o docs`.