JBoss BPM Suite Baggage Delivery Demo
=====================================
A baggage delivery service using BPM. A business friendly demo using form modeler, bpm process,
decisions table web, spreadsheet, dsl and bam.  

There are three options available to you for using this demo; local or containerized.


Option 1 - Install on your machine
----------------------------------
1. [Download and unzip.](https://github.com/effectivebpmwithjbossbpm/bpms-baggage-delivery-demo/archive/master.zip)

2. Download JBoss EAP & JBoss BPM Suite, add to installs directory (see installs/README).

3. Run 'init.sh' or 'init.bat' file. 'init.bat' must be run with Administrative privileges.

4. Start JBoss BPMS Server by running 'standalone.sh' or 'standalone.bat' in the <path-to-project>/target/jboss-eap-6.4/bin directory.

5. Login to [http://localhost:8080/business-central](http://localhost:8080/business-central)

    ```
     - login for admin and other roles (u:erics / p:bpmsuite1!)
    ```


Option 2 - Generate containerized installation
----------------------------------------------
The following steps can be used to configure and run the demo in a container

1. [Download and unzip.](https://github.com/effectivebpmwithjbossbpm/bpms-baggage-delivery-demo/archive/master.zip)

2. Download JBoss EAP & JBoss BPM Suite, add to installs directory (see installs/README).

3. Copy contents of support/docker directory to the project root.

4. Build demo image.

	```
	docker build -t effectivebpmwithjbossbpm/bpms-baggage-delivery-demo .
	```
5. Start demo container.

	```
	docker run -it -p 8080:8080 -p 9990:9990 -p 8001:8001 effectivebpmwithjbossbpm/bpms-baggage-delivery-demo
	```

Login to http://localhost:8080/business-central (u:erics / p:bpmsuite1!)

Read-write access to container JBoss BPM Suite git repo is available through ssh:

   ```
   $ git clone ssh://erics@localhost:8001/BagService

   Cloning into 'BagService'...
   The authenticity of host '[localhost]:8001 ([::1]:8001)' can't be established.
   DSA key fingerprint is SHA256:ok9ukS2j16
   Are you sure you want to continue connecting (yes/no)? yes

   Warning: Permanently added '[localhost]:8001' (DSA) to the list of known hosts.
   Password authentication
   Password: bpmsuite1!

   Receiving objects: 100% (1465/1465), 226.80 KiB | 0 bytes/s, done.
   remote: Total 1465 (delta 0), reused 1465 (delta 0)
   Resolving deltas: 100% (694/694), done.
   ```




Running the demo
----------------
Build the project then kick off the process. An initial form will show for first name, last name, 
flyer status {None, Bronze, Silver, Gold}, Country Code, Zip if in US and number of bags lost.  

If in US, will pass in zip code to zip code web service. Only the following zip codes will return 
states. Any other zip will return Texas.

	  //Park Ridge, IL
		zipCodes.put("60068", "IL");
		
		//Honolulu, HI
		zipCodes.put("96801", "HI");
		
		//New York, NY
		zipCodes.put("10001", "NY");
		
		//Bethel, AK
		zipCodes.put("99559", "AK");

After zip code service is called, the state field on passenger is used in web decision table to 
determine the cost of shipping.  Next, a DSL is used to figure out a surcharge for AK and HI.  

If not in US, county code is looked up in spreadsheet decision table and sets shipping and surcharge.

Finally, DSL is used to apply a promotion of free shipping and surcharge if passenger is of status Gold. 
You are then shown the route the process took and the variables to confirm shipping and surcharges are correct.

See support directory for included spreadsheet decision table ( NonUSShippingCost.xls) and docs directory for 
an overview presentation of the project.


Supporting Articles
-------------------
- [7 Steps to Your First Process with JBoss BPM Suite Starter	Kit](http://www.schabell.org/2015/08/7-steps-first-process-jboss-bpmsuite-starter-kit.html)

- [JBoss BPM Baggage Delivery is Helping Travelers with Lost Bags](http://www.schabell.org/2015/03/jboss-bpmsuite-helping-travelers-with-lost-bags.html)


Released versions
-----------------
See the tagged releases for the following versions of the product:

- v1.6 - Forked back from JBoss Demo Central for updating to JBoss BPM Suite 6.4.0, JBoss EAP 7.0.0, baggage delivery demo installed locally or containerized.

- v1.5 - JBoss BPM Suite 6.2.0-BZ-1299002 on JBoss EAP 6.4.4 and baggage delivery demo installed.

- v1.4 - JBoss BPM Suite 6.2.0, JBoss EAP 6.4.4 and OSE aligned containerization.

- v1.3 - JBoss BPM Suite 6.2.0, JBoss EAP 6.4.4 and baggage delivery demo installed.

- v1.2 - JBoss BPM Suite 6.1 and baggage delivery demo installed.

- v1.1 - JBoss BPM Suite 6.0.3 and baggage delivery demo installed with optional generation of containerized installation.

- v1.0 - JBoss BPM Suite 6.0.3 and baggage delivery demo installed.

![Baggage Process](https://github.com/effectivebpmwithjbossbpm/bpms-baggage-delivery-demo/blob/master/docs/demo-images/baggage-process.png?raw=true)

![Lost Form](https://github.com/effectivebpmwithjbossbpm/bpms-baggage-delivery-demo/blob/master/docs/demo-images/baggage-lost-form.png?raw=true)
