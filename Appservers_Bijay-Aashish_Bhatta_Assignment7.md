1.

-**Installing Glassfish 6:**
    psyphernix@psyphernix:~$ sudo apt update
    psyphernix@psyphernix:~$ sudo apt install openjdk-11-jdk
    psyphernix@psyphernix:~$ wget https://github.com/eclipse-ee4j/glassfish/releases/download/6.2.2/glassfish-6.2.2.zip
    psyphernix@psyphernix:~$ sudo mv glassfish-6.2.2.zip /opt/
    psyphernix@psyphernix:~$ cd /opt
    psyphernix@psyphernix:/opt$ sudo unzip glassfish-6.2.2.zip
    psyphernix@psyphernix:/opt$ sudo vim /etc/systemd/system/glassfish.service
    psyphernix@psyphernix:~$ sudo systemctl daemon-reload
    psyphernix@psyphernix:~$ sudo systemctl enable glassfish
    psyphernix@psyphernix:~$ sudo systemctl start glassfish
    psyphernix@psyphernix:~$ /opt/glassfish6.2.2/bin/asadmin enable-secure-admin
    export PATH=/opt/glassfish6.2.2/bin:$PATH
    
Contents of glassfish.service
    [Unit]
    Description = GlassFish Server v6.2.2
    After = syslog.target network.target

    [Service]
    ExecStart=/opt/glassfish6.2.2/bin/asadmin start-domain
    ExecReload=/opt/glassfish6.2.2/bin/asadmin restart-domain
    ExecStop=/opt/glassfish6.2.2/bin/asadmin stop-domain
    Type = forking

    [Install]
    WantedBy = multi-user.target
    
- **Creating a demo Java 11 Servelet application with Maven:**
