
# THIS IS ALPHA - DO NOT USE IN PRODUCTION ENVIRONMENT 




# Nextant

What it does right now:
- When uploaded to the cloud, text file are extracted to the Solr Server.
- It add an owner filter to solr documents so each user search in its own library.
- Use the Solr server when using the searchbox in the files App of your nextcloud.

What it does right now and should not:
- display results behind an element so it can't be clicked.

What it should do in the future:
- remove documents from the Solr Server when they are deleted from the cloud.
- documents should be kept on the Solr Server while the original file is still in the trashbin.
- testing your setup from the administration page
- extract files with an ./occ command
- extract more format (docx, pdf, ...)
- link to the right file when searching
- have a better display and a better indexing of the results



## Installation
# THIS IS ALPHA - DO NOT USE IN PRODUCTION ENVIRONMENT 

- Download the .zip, unzip and place this app in **nextcloud/apps/**,
- Build the app,
- Enable the app in the app list,
- Few setup in the administration page.

(You will also need an apache Solr server running somewhere, with Solr-Cell plugins.)

- install the solr on debian-like

	apt-get install solr-tomcat7

- Verify the version you just installed (3.6.2 on Jessie)

	apt-cache showpkg solr-tomcat7 

- Download the plugins for your version, and finish the installation using this [documentation](https://wiki.debian.org/Solr)
- Edit the **schema.xml** config file and add those four (4) lines inside the **fields /fields**

```xml
<fields>
...

   <field name="nextant_owner" type="text_general" indexed="true" stored="true"/>
   <field name="nextant_deleted" type="boolean" indexed="true" stored="true"/>
   <field name="nextant_shared" type="boolean" indexed="true" stored="true"/>
   <field name="nextant_sharing" type="text_general" indexed="true" stored="true" multiValued="true"/>
   
...
</fields>
```

## Building the app

The app can be built by using the provided Makefile by running:

    make

This requires the following things to be present:
* make
* which
* tar: for building the archive
* curl: used if phpunit and composer are not installed to fetch them from the web
* npm: for building and testing everything JS, only required if a package.json is placed inside the **js/** folder

The make command will install or update Composer dependencies if a composer.json is present and also **npm run build** if a package.json is present in the **js/** folder. The npm **build** script should use local paths for build systems and package managers, so people that simply want to build the app won't need to install npm libraries globally, e.g.:

**package.json**:
```json
"scripts": {
    "test": "node node_modules/gulp-cli/bin/gulp.js karma",
    "prebuild": "npm install && node_modules/bower/bin/bower install && node_modules/bower/bin/bower update",
    "build": "node node_modules/gulp-cli/bin/gulp.js"
}
```
