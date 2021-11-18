1. **GlassFish**

- **Installing Glassfish 6 and changing port to 8088:**

        $ sudo apt update
        $ sudo apt install openjdk-11-jdk
        $ wget https://github.com/eclipse-ee4j/glassfish/releases/download/6.2.2/glassfish-6.2.2.zip
        $ sudo mv glassfish-6.2.2.zip /opt/
        $ cd /opt
        $ sudo unzip glassfish-6.2.2.zip
        $ sudo vim /etc/systemd/system/glassfish.service
        $ sudo systemctl daemon-reload
        $ sudo systemctl enable glassfish
        $ sudo systemctl start glassfish
        $ sudo /opt/glassfish6/bin/asadmin enable-secure-admin
        $ export PATH=/opt/glassfish6/bin:$PATH
        $ sudo ufw allow 4848
        $ sudo ufw allow 8080
        $ sudo nano /opt/glassfish6/glassfish/domains/domain1/config/domain.xml
        $ sudo ufw deny 8080
        $ sudo ufw allow 8088
        $ sudo systemctl restart glassfish.service
    
Contents of glassfish.service

        [Unit]
        Description = GlassFish Server v6.2.2
        After = syslog.target network.target

        [Service]
        ExecStart=/opt/glassfish6/bin/asadmin start-domain
        ExecReload=/opt/glassfish6/bin/asadmin restart-domain
        ExecStop=/opt/glassfish6/bin/asadmin stop-domain
        Type = forking

        [Install]
        WantedBy = multi-user.target
    
- **Creating a demo Java 11 Servelet application with Maven:**

        $ sudo apt install maven
        $ sudo update-alternatives --config java
        $ mvn archetype:generate -DgroupId=psyphernix.io -DartifactId=HelloWorldTest -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false
        
- **Generating war package:**
        
        $ sudo mvn compile war:war
        
- **Deploying war using glassfish server:**

        $ sudo /opt/glassfish6/bin/asadmin deploy /home/psyphernix/HelloWorldTest/target/HelloWorldTest.war
        
 ***Note: All snapshots related to Maven are in [1. Maven](/1.%20Maven/).***
        
2. **Gunicorn**

- **Creating Django starter project in a separate virtual environment:**

        $ python3.9 -m venv webapp
        $ source webapp/bin/activate
        $ sudo python3.9 -m pip install django
        $ sudo python3.9 -m pip install gunicorn
        $ django-admin startproject webapp
        $ python3.9 manage.py startapp firstapp
        
- **Deploying three instances of gunicorn in port 8089:**

        $ sudo vim /home/psyphernix/webapp/webapp/webapp/settings.py
        $ sudo firewall-cmd --permanent --add-port=8089/tcp
        $ sudo firewall-cmd --reload
        $ python3.9 manage.py makemigrations
        $ python3.9 manage.py migrate
        $ python3.9 manage.py
        $ python3.9 -m gunicorn webapp.wsgi:application --workers 3 -b 127.0.0.1:8089
        
- **Dumping access log in a file in non-default pattern:**

        $ python3.9 -m gunicorn webapp.wsgi:application --access-logfile gunicornaccess.log -w 3 -b 127.0.0.1:8089 --capture-output
        
- **Dumping error log in a file:**

        $ python3.9 -m gunicorn webapp.wsgi:application --error-logfile gunicornerror.log -w 3 -b 127.0.0.1:8089 --capture-output

***Note: All snapshots related to Maven are in [2. Gunicorn](/2.%20Gunicorn/).***
