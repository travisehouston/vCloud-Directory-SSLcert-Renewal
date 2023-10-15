# vCloud-Directory-SSLcert-Renewal
This is a document on how to renew VMware Cloud Director SSL Certificates

# Prerequisites
To verify that this is the relevant procedure for your environment needs, familiarize yourself with SSL Certificate Creation and Management of the VMware Cloud Director Appliance.

If you are renewing the certificate for the primary node in a database high availability cluster, run the opt/vmware/vcloud-director/bin/cell-management-tool cell -m command of the cell management tool place all other nodes in maintenance mode and to prevent data loss. See Managing a Cell.
If FIPS mode is enabled, the root password of the appliance must contain 14 or more characters. See Change the Root Password of the VMware Cloud Director Appliance 10.4.1 or Later.

# Procedure
Log in directly or SSH to the OS of the VMware Cloud Director appliance as root.

To stop the VMware Cloud Director services, run the following command.

1. /opt/vmware/vcloud-director/bin/cell-management-tool -u administrator cell --shutdown

Generate new self-signed certificates for the database and appliance management UI or for the HTTPS communication, the database, and appliance management UI.
Generate self-signed certificates only for the embedded PostgreSQL database and the VMware Cloud Director appliance management UI, run:

2. /opt/vmware/appliance/bin/generate-certificates.sh <root-password> --skip-vcd-certs

This command automatically puts into use the newly generated certificates for the embedded PostgreSQL database and the appliance management UI. 

The PostgreSQL and the Nginx servers restart.

Generate new self-signed certificates for HTTPS communication of VMware Cloud Director in addition to certificates for the embedded PostgreSQL database and the appliance management UI.

# Run the following command:

1. /opt/vmware/appliance/bin/generate-certificates.sh <root-password>

If you are not using CA-signed certificates, run the commands to import the newly generated self-signed certificates to VMware Cloud Director.

2. /opt/vmware/vcloud-director/bin/cell-management-tool certificates -j --cert /opt/vmware/vcloud-director/etc/user.http.pem --key /opt/vmware/vcloud-director/etc/user.http.key --key-password root_password

# Restart the VMware Cloud Director service.

3. service vmware-vcd start

These commands automatically put into use the newly generated certificates for the embedded PostgreSQL database and the appliance management UI. The PostgreSQL and the Nginx servers restart. The commands generate a new, self-signed SSL certificate /opt/vmware/vcloud-director/etc/user.http.pem with private key /opt/vmware/vcloud-director/etc/user.http.key, which are used in Step 1.

# Results
The renewed self-signed certificates are visible in the VMware Cloud Director user interface.

The new PostgreSQL certificate is imported to the VMware Cloud Director truststore on other VMware Cloud Director cells the next time the appliance-sync function runs. The operation can take up to 60 seconds.

# What to do next
If necessary, a self-signed certificate can be replaced with a certificate signed by an external or internal certificate authority.
