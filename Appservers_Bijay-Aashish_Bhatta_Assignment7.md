1.

- **Installing Glassfish 6:**

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
        $ sudo export PATH=/opt/glassfish6/bin:$PATH
    
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
