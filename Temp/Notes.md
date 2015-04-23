WAS has two namespaces:
Global Namespace: this CosNaming namespace
ejblocal:
http://www-01.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.nd.doc/ae/cejb_bindingsejbfp.html?cp=SSAW57_8.5.5%2F1-3-0-11-3-1-0


dumpNameSpace.bat -port 9809 -root cell -report long >> C:/temp/jndi.txt
* dumpNameSpace class exists in 'com.ibm.ws.ejb.thinclient_8.5.0.jar'.  You can run from 

createEJBStubs.bat c:/temp/MyEAR.ear –updatefile c:/temp/MyEJB_Client.jar

Different initial references will give different starting root context: NameService, NameServiceServerRoot, ...etc

Write about the dumpNameSpace file content:
Every context in the tree has a unique id field
Each entry can be :
a) CosNming Context: It has binding type is ncontext (naming context).  This can be:
	a.1) existing context.
	a.2) linked to an existing context.
	a.3) linked through INS URL (corbaloc or corbaname) to a context existing in another naming server 
b) Object (Corba distributed object): These are deployed EJBs with remote interfaces and IBM supplied remote objects like 'EJBFactory'
c) Java Object:
	


* WAS: for a deployed EJB, WAS creates a 'EJBFactory' object and binds it in the namespace under
/nodes/Node01/servers/server1/com.ibm.websphere.ejbcontainer
??Study this object and try to call its methods

* The WAS Jar : com.ibm.ws.ejb.thinclient_8.5.0.jar has all stubs of the WAS remote objects. e.g: 'EJBFactory', 'WsnNameService', ..etc

Java ORB Portability API: stub and sekeleton generated by one tool (vendor) will work with any ORB implementation.
The Stub and Sekeleton delegate call to Delegate class which has the vendor specific implementation.

The deleagte has this method: _is_local which checks if the servant is local to the ORB instance and provides optimized
call.  All stub implementation make use of this.
https://books.google.com.sa/books?id=Vk3REMF9lIAC&pg=PA197&lpg=PA197&dq=java+corba+delegate&source=bl&ots=129mmF1Cg8&sig=FCUHKTv9ZG8YJn9POIddNxZh2MU&hl=en&sa=X&ei=dWjcVLz9N4P5UrvrgJgF&ved=0CE8Q6AEwCQ#v=onepage&q=java%20corba%20delegate&f=false

* IBM ORB Trace and ORBRas trace
ORBRas
RAS: Reliability Availability and Serviceability

* The service that JNDI initial context and lookup call:
1- WsnNameService.  has one operation 'getProperties'.  it does not require authentication.  It returns a list of
naming servers contexts and their IORs
