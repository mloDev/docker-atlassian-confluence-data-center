# docker-atlassian-confluence-data-center

Start an [Atlassian Confluence® Data Center](https://de.atlassian.com/enterprise/data-center) with Docker for local testing during plugin development.
It starts a PostgreSQL Database, several Confluence® cluster nodes and Apache2 HTTPD as sticky session loadbalancer. The shared confluence-home is handled via a shared Docker volume. This is not meant to be used in production! The cluster is designed to not be persistent, meaning, once you shut it down, all data is lost. See it as the Data-Center version of [`atlas-run-standalone`](https://developer.atlassian.com/docs/developer-tools/working-with-the-sdk/command-reference/atlas-run-standalone).

[![](https://codeclou.github.io/docker-atlassian-confluence-data-center/img/manage-confluence-cluster-logo.svg)](https://github.com/codeclou/docker-atlassian-confluence-data-center)


Please choose the Confluence version you want to run:

|Confluence Version | Status | Loadbalancer URL | 3rdparty Versions |
|-------------------|--------|------------|-----------------|
| **⇨ [6.2.0](https://github.com/codeclou/docker-atlassian-confluence-data-center/tree/master/6.2.0)** | :white_check_mark:&nbsp;current | http://confluence-cluster-620-lb:50620/ | [postgres:9.4](https://hub.docker.com/_/postgres/) <br> [java:8u131](https://github.com/codeclou/docker-atlassian-base-images/blob/confluence-6.2.0/Dockerfile) |
| **⇨ [6.1.2](https://github.com/codeclou/docker-atlassian-confluence-data-center/tree/master/6.1.2)** | :white_check_mark:&nbsp;current | http://confluence-cluster-612-lb:50612/ | [postgres:9.4](https://hub.docker.com/_/postgres/) <br> [java:8u131](https://github.com/codeclou/docker-atlassian-base-images/blob/confluence-6.1.2/Dockerfile) |
| **⇨ [6.1.1](https://github.com/codeclou/docker-atlassian-confluence-data-center/tree/master/6.1.1)** | :red_circle:&nbsp;deprecated | http://confluence-cluster-611-lb:50611/ | [postgres:9.4](https://hub.docker.com/_/postgres/) <br> [java:8u131](https://github.com/codeclou/docker-atlassian-base-images/blob/confluence-6.1.1/Dockerfile) |


**Please Note:**
 * I do not provide support. If you have questions on how to run Confluence and/or Confluence Data Center, please ask in the
[Atlassian Community](https://community.atlassian.com/).

-----

&nbsp;

### Running the Confluence Data Center Plugin Validator

If the cluster is finally started up, you can run the [Confluence Data Center Plugin Validator](https://developer.atlassian.com/confdev/development-resources/confluence-developer-faq/how-do-i-ensure-my-add-on-works-properly-in-a-cluster/confluence-data-center-plugin-validator).

But before you do, please verify:

 * (1) [Atlassian Universal Plugin Manager](https://marketplace.atlassian.com/plugins/com.atlassian.upm.atlassian-universal-plugin-manager-plugin/server/overview) (UPM) is up to date
 * (2) All Cluster Nodes are active
 * (3) Your plugin is installed successfully

Now connect to one of the cluster nodes via:

```bash
docker exec -i -t confluence-cluster-612-node1 bash
```
Now inside the container run

```bash
cd /work
# download
curl -jkSL -o /work/cdc-plugin-validator-1.0.0.jar \
https://developer.atlassian.com/confdev/files/29466176/29525066/1/1407451934928/cdc-plugin-validator-1.0.0.jar
# exec validator
java -jar cdc-plugin-validator-1.0.0.jar \
     -installation /confluence/atlassian-confluence-latest \
     -dbuser confluence -dbpassword confluence \
     -dburl jdbc:postgresql://confluence-cluster-612-db:5432/confluence \
     -dbdriver org.postgresql.Driver \
     -dbfile /confluence/atlassian-confluence-latest/confluence/WEB-INF/lib/postgresql-9.4.1212.jar
```

You should get an output for your Plugin. For one of my Plugins the output looks like so:

```txt
...
Summary:
========
PASS: ./plugins/fr.spectrumgroupe.confluence.plugins.formatting.macros--plugin.8952733569564445853.formatting.macros-1.0.13.jar.report
PASS: ./plugins/com.atlassian.support.stp--stp-3.10.4.jar.report
PASS: ./plugins/com.atlassian.upm.atlassian-universal-plugin-manager-plugin--plugin.1480744321619799648.atlassian-universal-plugin-manager-plugin-2.22.2.jar.report
PASS: ./plugins/com.atlassian.confluence.plugins.confluence-healthcheck-plugin--confluence-healthcheck-plugin-2.4.1.jar.report
PASS: ./plugins/confluencemavenreleaseinfomacro--plugin_6773597919257421869_confluence-maven-release-info-macro-1.5.2.jar.report
```

Now you can inspect the reports for further information.

-----

&nbsp;

### Trademarks and Third Party Licenses

 * **Atlassian Confluence®**
   * Atlassian® and Confluence® are registered [trademarks of Atlassian Pty Ltd](https://de.atlassian.com/legal/trademark).
   * Please check yourself for corresponding Licenses and Terms of Use at [atlassian.com](https://atlassian.com).
 * **Oracle Java JDK 8**
   * Oracle and Java are registered [trademarks of Oracle](https://www.oracle.com/legal/trademarks.html) and/or its affiliates. Other names may be trademarks of their respective owners.
   * Please check yourself for corresponding Licenses and Terms of Use at [www.oracle.com](https://www.oracle.com/).
 * **Docker**
   * Docker and the Docker logo are trademarks or registered [trademarks of Docker](https://www.docker.com/trademark-guidelines), Inc. in the United States and/or other countries. Docker, Inc. and other parties may also have trademark rights in other terms used herein.
   * Please check yourself for corresponding Licenses and Terms of Use at [www.docker.com](https://www.docker.com/).
 * **PostgreSQL**
   * PostgreSQL is a [registered trademark of the PostgreSQL Community Association of Canada](https://wiki.postgresql.org/wiki/Trademark_Policy).
   * Please check yourself for corresponding Licenses and Terms of Use at [www.postgresql.org](https://www.postgresql.org/).
 * **Ubuntu**
   * Ubuntu and Canonical are registered [trademarks of Canonical Ltd.](https://www.ubuntu.com/legal/short-terms)
 * **Apple**
   * macOS®, Mac and OS X are [trademarks of Apple Inc.](http://www.apple.com/legal/intellectual-property/trademark/appletmlist.html), registered in the U.S. and other countries.

-----

&nbsp;

### License

[MIT](https://github.com/codeclou/docker-atlassian-confluence-data-center/blob/master/LICENSE) © [Bernhard Grünewaldt](https://github.com/clouless)
