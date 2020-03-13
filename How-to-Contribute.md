# Writing Content
Before you begin, follow the steps to install **dependencies and pre-commit hooks** under ["Developing"](https://github.com/splunk/security-content#developing). 

1. Select the content [piece](https://github.com/splunk/security-content#content-parts) you want to write. 
2. Copy an example and edit it to suit your needs. At a minimum, you must write a [story](stories/), [a detection search](detections/), and an [investigative search](investigations/).
3. Make a pull request. The pull request will trigger CircleCI, a continuous-integration app thatintegrates with a VCS and automatically runs a series of steps every time that it detects a change to your repository. A CircleCI build consists of a series of steps, usually Dependencies, Testing, and Deployment. If your tests pass, you're good to go! If the CircleCI check fails, refer to [troubleshooting](https://github.com/splunk/security-content#troubleshooting).  

For a more detailed explanation on how to contribute to the project, please see ["Contributing"](#Contributing)