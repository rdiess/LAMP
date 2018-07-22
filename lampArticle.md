

<!-- Intro to Lamp -->
<h1> Monitoring the LAMP stack with Datadog  </h1>

<div>

<p>
The LAMP stack is a set of open-source software used for creating websites and web applications. Lamp is named for its four original components — Linux, Apache, MySQL, and PHP — and, though evolving to include alternatives like Python and Perl, it has retained its open-source nature. LAMP's cost-effective and flexible approach to web infrastructure makes it the most popular solution stack for hosting websites and web apps.
</p>

<p>
By providing real-time collection and vizualization of key metrics across each layer of the LAMP stack, Datadog offers a flexible and customizeable way to monitor your web app's performance and usage.
</p>

<p>

<figure>
    <img src='keymetrics.png' alt='missing' />
    <figcaption>
  <font size = "1">
  A custom dashboard created with Datadog
  </font>
</figcaption>
</figure>

</p>

</br>
<p>
This post will walk you through the integration and configuration of each LAMP stack layer. You'll learn how to:
</p>

<ul>
  <li>Install the Datadog agent</li>
  <li>Integrate each layer of your LAMP stack with Datadog</li>
  <li>Configure Datadog to collect the desired metrics</li>
  <li>Customize, organize, and optimize your monitoring experience</li>
</ul>

<!-- Datadog Installation -->
<h2>Installing the Datadog Agent</h2>

<div>

<p>
After you've created your
<a href="url">Datadog account</a>
(or signed up for a 14-day
<a href="https://www.datadoghq.com/">free trial</a>), you'll recieve an application key that you'll use to install the
<a href = "https://docs.datadoghq.com/agent/"> Datadog agent.</a> You can find the key in your <a href = "https://app.datadoghq.com/account/settings#api"> account </a>.


</p>

<p>
Run the following script to install the agent on your machine:

<code>
DD_API_KEY=<YOUR_API_KEY> bash -c "$(curl -L https://raw.githubusercontent.com/DataDog/datadog-agent/master/cmd/agent/install_script.sh)"
</code>
</p>

<p>
To check the success of the installation, run:

<code>
sudo datadog-agent status
</code>
</p>

<p>
If the installation was successful, you should see information about the running agent:

<code>
Getting the status from the agent. ============== Agent (v6.2.0) ============== Status date: 2018-05-16 19:45:31.773123 UTC Pid: 26471 Python Version: 2.7.12 Logs: Check Runners: 1 Log Level: info Paths ===== Config File: /etc/datadog-agent/datadog.yaml conf.d: /etc/datadog-agent/conf.d checks.d: /etc/datadog-agent/checks.d [...]
</code>
</p>

<br />
<p>
After that, you're ready to view your host's metrics in real-time via the Datadog dashboard: go to Infrastructure, HostMap, and click on the system panel that appears after your host has been selected.

<figure>

<img src = "HostSystemMetrics.png">
</p>

<p>

<figcaption>
<font size = "1">
the host's system metrics:
<br />
Infrastructure->HostMap->your Host->System
</font>
</figcaption>
</figure>

<br />

</p>

<p>
Now that you've installed the Datadog Agent on your host, you can begin integrating the rest of your lamp stack. The remainder of the article will walk you through the steps necessary to use the agent to monitor each software component in LAMP. Check out the
<a href = "https://docs.datadoghq.com/"> documentation
</a>
to see a full list of
<a href = "https://docs.datadoghq.com/agent/basic_agent_usage/amazonlinux/">commands
</a>
for the Datadog agent.
</p>

</div>

<!-- Apache Integration, Configuration, and Monitoring -->
<br />
<h2>
Monitoring the Apache Web Server
</h2>

<div>

<h3>
Integration
</h3>

<p>
 Select the <a href = "https://app.datadoghq.com/account/settings#integrations/apache"> Apache tile </a> in the <a href = "https://app.datadoghq.com/account/settings#integrations/apache"> Integrations </a> section of the Datadog website. Following the directions in the tile, you may need to install
<a href = "http://httpd.apache.org/docs/current/mod/mod_status.html"> mod-status on your Apache server.
</a>

</p>

<h4>
Apache's status Module
</h4>

<p>
Your server's status page should be available at:

<br />
<code>
http://yourpage@example.com/server-status
</code>

<br />

If this link throws a forbidden access error, then you'll need to <a href = "https://www.datadoghq.com/blog/collect-apache-performance-metrics/#apache-s-status-module">
enable mod-status
</a> manually.

</p>

<h3>
Configuration
</h3>

<p>
To configure Apache to report metrics to the Datadog agent, navigate to your a <span title = "select your platform and then go to the 'configuration' section for instructions on how to navigate to your particular conf.d directory file.">

<a href = "https://docs.datadoghq.com/agent/">
apache.d/conf.yaml.example
</a>

</span>
Create a conf.yaml file with the following code:
<br />
<code>
sudo cp conf.yaml.example conf.yaml
</code>
</p>

<br />
<p>
Edit the new conf.yaml file to match the following configuration:

</p>

<p>

<img src = "apacheconfig.png">

</p>

<br/>
When the agent runs, it will look for a conf.yaml file, and begin pulling metrics from Apache when it finds it.


<p>
To check that the agent is collecting data from Apache run the following two commands to:
</p>

<br />
<p>
restart the agent:

<br />
<code>
sudo service datadog-agent restart
</code>

<br />
<br />
check that Apache has been integrated successfully

<br />
<code>
sudo service datadog-agent restart
</code>

<br />
<br />
If Apache has been integrated succesfully, the output should contain a section similar to this:

<br />
<code>
Checks
======
<br />
  [...]
<br />
<br />
<br />
  apache
  <br />
  ------
  <br />
      - instance #0 [OK]
      - <br />
      - Collected 8 metrics & 0 events
</code>

</p>

<br />
<h3>
Monitoring
</h3>

<p>
Now that you have your Apache server integrated and configured properly, visit your
<a href = ""> Apache dashboard </a>

on Datadog to view your server's metrics in real time.
</p>

<figure>

<img src = "ApacheDash.png">
</p>
<figcaption>
<font size = "1">
The Apache Dashboard:
<br />
Dashboards -> DasboardList -> Apache
</font>
</figcaption>
</figure>

<br />

</div>

<!-- MySQL Integration, Configuration, and Monitoring -->
<br />
<h2>
Monitoring the MySQL Database
</h2>

<div>

<h3>
Integration
</h3>

<p>
 Select the
 <a href = "https://app.datadoghq.com/account/settings#integrations/mysql"> MySQL tile
 </a> in the <a href = "https://app.datadoghq.com/account/settings#integrations/apache"> Integrations
 </a> section of the Datadog website.
Following the directions on the tile, use the following commands to:
</p>

<p>
create a Datadog user on your MySQL server

<br />
<code>
sudo mysql -e "CREATE USER 'datadog'@'localhost' IDENTIFIED BY 'yourpassword';"
</code>

</p>

<p>
grant that user replication rights:

<br />
<code>
sudo mysql -e "GRANT REPLICATION CLIENT ON *.* TO 'datadog'@'localhost' WITH MAX_USER_CONNECTIONS 5;"
</code>

</p>

<p>
grant all additional permissions in order to obtain the full metrics catalog:

<br />
<code>
sudo mysql -e "GRANT PROCESS ON *.* TO 'datadog'@'localhost';"
</code>

<br />
<code>
sudo mysql -e "GRANT SELECT ON performance_schema.* TO 'datadog'@'localhost';"
</code>

</p>

<p>
Use the following commands to check if:

<br />
<br />
a Datadog user with replication rights has been created on the MySQL server:
<br />

<code>
mysql -u datadog --password='TFs1v1AEY9>kv84btq29n6Yy' -e "show status" | \
grep Uptime && echo -e "\033[0;32mMySQL user - OK\033[0m" || \
echo -e "\033[0;31mCannot connect to MySQL\033[0m"
mysql -u datadog --password='TFs1v1AEY9>kv84btq29n6Yy' -e "show slave status" && \
echo -e "\033[0;32mMySQL grant - OK\033[0m" || \
echo -e "\033[0;31mMissing REPLICATION CLIENT grant\033[0m"
</code>

<br />

Any additional priveleges have been granted:

<br />
<code>
mysql -u datadog --password='TFs1v1AEY9>kv84btq29n6Yy' -e "SELECT * FROM performance_schema.threads" && \
echo -e "\033[0;32mMySQL SELECT grant - OK\033[0m" || \
echo -e "\033[0;31mMissing SELECT grant\033[0m"
mysql -u datadog --password='TFs1v1AEY9>kv84btq29n6Yy' -e "SELECT * FROM INFORMATION_SCHEMA.PROCESSLIST" && \
echo -e "\033[0;32mMySQL PROCESS grant - OK\033[0m" || \
echo -e "\033[0;31mMissing PROCESS grant\033[0m"
</code>

</p>

<br />
<h3>
Configuration
</h3>

<p>
Just like Apache, to configure mySQL to report metrics to the Datadog agent, you'll need to navigate to your a <span title = "select your platform and then go to the 'configuration' section for instructions on how to navigate to your particular conf.d directory">

<a href = "https://docs.datadoghq.com/agent/">
mysql.d/conf.yaml.example
</a>
file. Create a conf.yaml file with the following code:
</span>


<br />
<code>
sudo cp conf.yaml.example conf.yaml
</code>

</p>

<p>
Edit the conf.yaml file to match the following configuration:

</br>

<img src = "mysqlconfig.png">

</p>

<p>


When the agent runs, it will look for a conf.yaml file, and begin pulling metrics from MySQL.
</p>

<p>
To check that the agent is collecting data from MySQL run the following two commands to:

<br />
restart the agent:

<br />
<code>
sudo service datadog-agent restart
</code>

<br />
check that mySQL has been integrated successfully
<br />
<code>
sudo service datadog-agent restart
</code>


<br />
If MySQL has been integrated succesfully, the output should contain a section similar to this:

<br />
<code>
Checks
======
<br />
  [...]
<br />
<br />
<br />
  mySQL
  <br />
  ------
  <br />
      - instance #0 [OK]
      - <br />
      - Collected 8 metrics & 0 events
</code>

</p>

<h3>
Monitoring
</h3>

<p>
Now that you have your MySQL server integrated and configured properly, visit your
<a href = ""> MySQL dashboard </a>
on Datadog to view your server's metrics in real time.
</p>

<figure>

<img src = "mysqldash.png">
</p>
<figcaption>
<font size = "1">
The MySQL Dashboard:
<br />
Dashboards -> DasboardList -> MySQL
</font>
</figcaption>
</figure>

<br />

</div>

<!-- Custom Dashboards -->

<div>
<br />
<h2> Custom Dashboards </h2>

<p>
Datadog dashboards allow you to gather customized vizualizations for any obtained metrics in one place. To create a custom dashboard, click on the "new dashboard" option in the Dashboards pane. Click on "add graphs" at the top of the page to drag and drop real-time vizualized metrics from all your integrations. For a full list of dashboard features, visit the
<a href = "https://www.datadoghq.com/dashboarding/">
Dashboarding page.
</a>
</p>

<img src = "customdash.png">
</p>
<figcaption>
<font size = "1">
A Custom Dashboard:
<br />
Dashboards -> New Dashboard
</font>
</figcaption>
</figure>

</div>

<!-- Wrap up & Call to Action -->

<div>
<br />
<h2> Full LAMP Observability with Datadog </h2>
<p>
The LAMP solution stack is the most popular approach for hosting web apps and websites. We've shown you just a few ways to collect and visualize key metrics from each layer of your LAMP stack.
<p>
Datadog integrates with more than 250 other tools and services to provide a comprehensive, customizeable, and intuitive monitoring experience. Start monitoring your LAMP stack today with a
<a href = "https://www.datadoghq.com/">
free trial.
</a>
</p>

</p>

</div>
