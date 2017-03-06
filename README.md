# Netapp zapi Modules
Version : 0.1

NetApp Manageability SDK release 5.6

Copyright Notice:
Copyright @ 1999-2015 NetApp,Inc. All rights reserved.

U.S. Government Rights -- Commercial Computer Software.
Government users are subject to the NetApp, Inc. standard license agreement 
and applicable provisions of the FAR and its supplements.


NetApp, the NetApp logo, and Go Further Faster are trademarks or registered
trademarks of NetApp, Inc. in the U.S. and/or other countries.

'''sh
from netapp import NaElement
from netapp import NaServer

s = NaServer('filer', 1, 21)
s.set_transport_type("HTTP")
s.set_port(80)
s.set_server_type("FILER")
s.set_style("LOGIN")
s.set_admin_user('admin', 'password')

api = NaElement("aggr-space-get-iter")

xi = NaElement("desired-attributes")
api.child_add(xi)

xi1 = NaElement("space-information")
xi.child_add(xi1)

xi1.child_add_string("aggregate","<aggregate>")
xi1.child_add_string("aggregate-size","<aggregate-size>")
xi1.child_add_string("physical-used","<physical-used>")
xi1.child_add_string("physical-used-percent","<physical-used-percent>")

xi2 = NaElement("query")
api.child_add(xi2)


xi3 = NaElement("space-information")
xi2.child_add(xi3)

xo = s.invoke_elem(api)
if (xo.results_status() == "failed") :
    print ("Error:\n")
    print (xo.sprintf())


aggrs= xo.child_get("attributes-list").children_get()

for aggr in aggrs:
    print ('name {} {}'.format(aggr.child_get_string("aggregate"),aggr.child_get_string("physical-used")))


'''