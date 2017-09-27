# opensips-clusterer
OpenSIPS Clusterer Experiment
## Summary:
This config utilizes the latest opensips-dev module clusterer's functions to send custom messages between the proxies and get soem responses back. 

## Objective:
The objective for this config file here is to demonstrate that OpenSIPS in a cluster can exchange user location information over Binary protocol. Previously other methods exist like shared DB, Shared Redis/memcache cluster, RabitMQ, Rest_client etc that enable proxies to share business logic between them but the problem I see is adding more software to assist SIP !? For example sending information over HTTP or MySQL or CACHE interfaces to exchange info that gets complicated otherwise for OpenSIPS to exchange within it's config file.

I wanted to keep everything within the OpenSIPS/SIP protocol and be free to send/receive whatever info I require w/o developing in different tools/languages. 

The new OpenSIPS Clustere module now enables me to send info over to a Cluster of OpenSIPS boxes and in turn receive replies to continue with the script. 

## Example:
I have about 20 SIP Proxies discovering each other via Clusterer module (No DB table used). The users are scattered globally and move frequently also expecting reliable communications with other users. The config file shared here updates the cluster anytime a user comes online or goes offline. Similarly any new proxy joining/re-joining the cluster, is unaware of the previously exchanged updates, broadcasts "WHEREIS:$avp(user)" to the whole cluster. Any proxy who has the user online replies to this new proxy and hence call is relayed->connected.

## BUT WHY !!
Once you review the config file, I'll ask if you find any external dependency on additional tools making a cluster or using another protocol to exchange data !? (redis used here is local cache only - for the matter an internal opensips hash could be used as well)

Someone mentioned that TM module already has the function to send a CUSTOM SIP method to some destined proxy - the problem is right here that, while I can do that, it will make my life difficult, if not hell, to add new nodes write code that loops over the list of proxies and send message ! felt very awkward to me to even think of doing it that way. Ther eis no SIP broadcast possible !

Next, Use Redis as cluster ! well I hate to make redis cluster that is distributed across the globe, I do not trust consistency of such cluster OR maybe my experiences are limited. Same goes for clustering a MySQL/PGSQL DB. 

Another possibility is using REST_CLIENT module to do rest_post() requests/updates... again why should I write code in PHP/Python/Ruby/GoLang and manage the webservices on distributed proxies !!

This becomes so simpler to spin up a simple opensips service and it automagically connects and performs with the cluster without anymore tools installed and cared for.




