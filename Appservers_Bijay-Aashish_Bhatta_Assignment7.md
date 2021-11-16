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

        $ sudo python3 -m venv Djangunicorn
        $ source Djangunicorn/bin/activate
        $ sudo python3 -m pip install django
        $ django-admin startproject djangoatleapfrog
        $ sudo nano /home/psyphernix/djangoatleapfrog/djangoatleapfrog/settings.py
        
- **Deploying three instances of gunicorn in port 8089:**

        $ sudo apt install gunicorn
        $ 
        
- **Dumping access log in a file in non-default pattern:**

        $
        
- **Dumping error log in a file:**

        $

