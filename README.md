# Elis Vendor Matching Connector

The connector matches extracted vendor name, address and VAT ID against a vendor database. 
Any of these data is sufficient for the right vendor match.

![Vendor Matching Connector](vendor_matching_connector.gif)

To set the connector up:

  * `sudo apt install python3-pip` and `pip3 install -r requirements.txt`
  * The access credentials for the PostgreSQL database are stored in `config.py` and `~/.pgpass` file.

To run the connector:
```
	python3 connector.py
```

To configure an elis queue to use the connector:
```
	curl -u ELIS_USERNAME:ELIS_PASSWORD -H 'Content-Type: application/json' -d '{"name": "Vendor Matching Connector", /
	"service_url": "SERVER_URL", "authorization_token":"AUTHENTICATION_TOKEN", /
	"queues":["https://api.elis.rossum.ai/v1/queues/QUEUE_NUMBER"]}' 'https://api.elis.rossum.ai/v1/connectors'
```
where:
  * ELIS_USERNAME = account you use to login to Elis
  * ELIS_PASSWORD = password to your Elis account
  * SERVER_URL = url path of the server where the vendor matching connector is run
  * AUTHENTICATION_TOKEN = the token Elis uses when accessing connector, stored in `config.py`.
  * QUEUE_NUMBER = number of the queue where the connector should run

For more information on configuration see 
<a href="https://api.elis.rossum.ai/docs/#overview">Elis Document Management API</a>.

To configure the schema for connector to work:
```
	elisctl queue change SCHEMA_ID -s example_schema.json
```

where:
  * SCHEMA_ID = id of the schema that the connector should be run on
  * example_schema.json = example schema that is set up to work with connector. Can be customized based on your needs.

For more information on working with elisctl and its download see 
<a href="https://github.com/rossumai/elisctl">elisctl Github page</a>.

To use the connector for production, run via HTTPS using, for example, Nginx proxy with Let's encrypt 
TLS/SSL certificate. 

To import testing vendor data to database:
```
    python3 import_vendor_data.py supportive_data/vendor_data_de.csv
```