# Dspace statisics generation

Run python 2 script `update_dspace_front_page_stat.py` periodically with crontab to create and update a file with dspace item views statistics.

By default script places statistics file into `/var/cache/tomcat/pops.xml` (this location be changed inside the script). This xml file is then served by any web server at `/static/pops.xml` (e.g.: http://datadoi.ut.ee/static/pops.xml). Finally, `Mirage2-TU` theme uses xslt to parse the contents of this file and display it on the Dspace landing page.

### Setting up tomcat to serve static files

If you want to serve statistics file with Tomcat you will need to (Ref. http://www.moreofless.co.uk/static-content-web-pages-images-tomcat-outside-war):


1. Add the following line to `tomcat/conf/server.xml`

      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">

      <Context docBase="/var/cache/tomcat" path="/static" />  <!-- added part -->

2. Make sure that /var/cache/tomcat and all files inside are owned by tomcat user
3. The file placed into `/var/cache/tomcat/file.txt` will be accessible via `/static/file.txt`







### Exmaples of dspace Solr queries:

**top 5 views**
http://localhost:8080/solr/statistics/select?q=type:2&fq=statistics_type:view&fq=-isBot:true&fl=uid+score+&df=id&indent=true&facet=true&facet.field=id&omitHeader=true&f.id.facet.limit=5

**last 7 days item views**
http://localhost:8080/solr/statistics/select?q=type:2&fq=statistics_type:view&fq=-isBot:true&fq=time:[NOW-7DAY%20TO%20NOW]

**top 5 views from last 7 days**
http://localhost:8080/solr/statistics/select?q=type:2&fq=statistics_type:view&fq=-isBot:true&fq=time:[NOW-7DAY%20TO%20NOW]&fl=uid+score+&df=id&indent=true&facet=true&facet.field=id&omitHeader=true&f.id.facet.limit=5

**all items**
http://localhost:8080/solr/search/select?&start=0&rows=4&q=search.resourcetype:2

**display handle only**
http://localhost:8080/solr/search/select?&start=0&rows=4&q=search.resourcetype:2&fl=handle

**handle of an item with a specific id**
http://localhost:8080/solr/search/select?q=search.resourcetype:2+AND+search.resourceid:5d46ad1d-171d-4af8-8b09-480e2df405d3&fl=handle&omitHeader=true






