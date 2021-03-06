# University of Arizona Libraries Customizations For LibApps

This repository contains customizations for LibApps for the University of Arizona Libraries.
The styles here are intended to match [UA Sites](https://uasites.arizona.edu/home).

## People

This repository is primarily intended for use by the members of Research & Learning, in particular [Jennifer Church-Duran](https://github.com/jchurchduran), [Leslie Sult](https://github.com/lsult), and [Nicole Hennig](https://github.com/nic-hennig).

It was originally developed by [Will Simpson](https://github.com/simpsonw)

## Initial setup

There are three CSS files generated by the build process in this repository: `azlist.css`, `libanswers.css`, and `libguides.css`.  All
three contain the styles needed for the overall layout of the site plus custom styles specific to that part of LibApps (the A-Z list for `azlist.css`, LibGuides that are related to the Main Library for `libguides.css`, and LibAnswers for `libanswers.css`).

Paste the following block of code in the "Custom JS/CSS" section under "Admin > Look & Feel" on **LibGuides**:

```
<link rel="stylesheet" type="text/css" href="https://ual-libapps.s3-us-west-2.amazonaws.com/azlist.css">
<script src="https://ual-libapps.s3-us-west-2.amazonaws.com/vendor.js"></script>
<script src="//v2.libanswers.com/load_chat.php?hash=07713bc057f66ebcdccd4dd1b4a2be3e"></script>
```

Make sure that "Do not include this code on guide edit page." and "Exclude system level JS/CSS code." are both checked and click
"Save".

Go to "Header/Footer/Tabs/Boxes", select "Display this HTML", and paste the contents of `html/header.html` into "Group Header". Ensure that "Exclude System Level Header" is checked, and click "Save".  Then paste the contents of `html/footer.html` into "Group Footer HTML", ensure that "Exclude system level footer is checked", and click "Save".

Paste the following block of code in the "Custom JS/CSS" section under "Admin > Groups > (Group Name) > Look & Feel" on **LibGuides**:


```
<link rel="stylesheet" type="text/css" href="https://ual-libapps.s3-us-west-2.amazonaws.com/libguides.css">
<script src="https://ual-libapps.s3-us-west-2.amazonaws.com/vendor.js"></script>
<script src="//v2.libanswers.com/load_chat.php?hash=07713bc057f66ebcdccd4dd1b4a2be3e"></script>
```

Again, make sure that "Do not include this code on guide edit page." and "Exclude system level JS/CSS code." are both checked and click
"Save".

Go to "Header/Footer/Tabs/Boxes", select "Display this HTML", and paste the contents of `html/header.html` into "Group Header". Ensure that "Exclude System Level Header" is checked and click "Save".

Then expand the "Footer" section of the page and paste the contents of `html/footer.html` into "Group Footer HTML". Ensure that "Exclude system level footer is checked" and click "Save".


Paste the following block of code in the "Custom JS/CSS" section under "Admin > FAQ Groups > (Group Name) > Look & Feel" on **LibAnswers**:

```
<link rel="stylesheet" type="text/css" href="https://ual-libapps.s3-us-west-2.amazonaws.com/libanswers.css">
<script src="https://ual-libapps.s3-us-west-2.amazonaws.com/vendor.js"></script>
<script src="//v2.libanswers.com/load_chat.php?hash=07713bc057f66ebcdccd4dd1b4a2be3e"></script>
```

Make sure that "Include system-level JS/CSS code on this group's pages", "Include system-level Header code on this group's pages", and "Include system-level Footer code on this group's pages" all have "No" selected and click "Save"

Paste the contents of `html/header.html` into "Custom Header", click the save button underneath the text box.  Then paste the contents of `html/footer.html` into "Custom Footer" and click the save button underneath that text box.


## Making changes

0. Test your changes out in a sandbox environment in the "Custom JS/CSS" box of the application you want to style.

1. Go to `src/css/` in this repository, click the edit icon, and add your CSS to the appropriate file:  

* For styles that should only affect the "Databases A-Z list", add the changes to `src/css/azlist.css`.  
* For styles that should affect all LibGuides from the Main Library, add the changes to `src/css/libguides.css`.  
* For styles that should affect LibAnswers, add the changes to `src/css/libguides.css`.  
* For styles that should affect all three, add the changes to `src/css/styles.css`.

2. Wait approximately 30 seconds to a minute for the build and deployment to finish.  You should be able to track the progress of this through the "Actions" tab on this Github repository.

3. The changes should be live.

## Technical Overview

This project uses Webpack to build assets and Amazon S3 (located in the Libraries production AWS account managed by TeSS) to serve them.  Github Actions are used to deploy to production on every commit/push to master (see `.github/workflows/main.yaml` for the full configuration).  Two Github Secrets need to be present for the deployment to work: `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.  These should be set the values specified in [Stache](https://stache.arizona.edu/new/entry/77512cf5d4d72baa96b10a8ea7721081).

### CORS

Since this project is providing assets that are supposed to be served on a domain other than the one they're hosted on, you have to ensure that your Cross-origin resource sharing (CORS) configuration for S3 allows this.  The necessary setup is documented in `cors.json`.  In the event that a new S3 bucket has to be provisioned to serve this project or the CORS configuration needs to be updated (e.g. to allow a new domain), you can set the CORS configuration using the following command:

```
aws s3api put-bucket-cors --profile <profile_name> --bucket <bucket_id> --cors-configuration file://cors.json
```

![Node.js CI](https://github.com/ualibraries/ual-libapps/workflows/Node.js%20CI/badge.svg)
