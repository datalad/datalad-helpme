# DataLad Helpme

This repository serves automatically generated issues by way of the [helpme](https://vsoch.github.io/helpme)
command line client. Thank you for your patience as we are developing and testing this
integration.

## Usage

This integration works with helpme versions 0.0.40 and later. Datalad will be
required to specify this repository base along with metadata to open an issue.
As an example:


```python
from helpme.main import get_helper
helper = get_helper(name="github")
```

You are required to provide the repository, title, and the body text for the issue.
In the example below, we use <a href="https://github.com/datalad/datalad" target="_blank">datalad</a> 
to generate a dictionary of output, and we will submit it to the repository `datalad/datalad-helpme`.
Here is an example of submitting without a token (meaning the browser url is opened and the user
must click submit. This is done simply by not exporting `HELPME_GITHUB_TOKEN` to the environment.

```python
from helpme.main import get_helper
from datalad.utils import swallow_outputs
from datalad.api import wtf
import sys

repo = "datalad/datalad-helpme"
title = "Test issue opened manually by helpme"
helper = get_helper(name="github")

with swallow_outputs() as cmo:
    wtf(decor="html_details")
    body = """
## What is the problem?
<!-- Please write a few sentences about the issue-->
## What steps will reproduce the problem?
<!-- What triggered this error? -->
## Is there anything else that would be useful to know in this context?
<!-- Have you had any success using DataLad before? (to assess your expertise/prior luck.  We would welcome your testimonial additions to https://github.com/datalad/datalad/wiki/Testimonials as well)-->
%s""" % cmo.out
```

We will want to add a hash of the problem to this, for the CI to check if the issue
has been previously opened.

Next, let's run the helper headlessly. Note that this will currently still confirm the
environment variables being sent from the user.

```python
issue = helper.run_headless(repo=repo, body=body, title=title)
```

The above will still require confirmation to send the user environment,
and then will post the issue to `datalad/datalad-support`. The user
can fill in the sections to complete the issue.
