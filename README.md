# google-analytics-events-companies
In early February, Google performed a product update of Google Analytics. One of the most important changes is the removal of data from the report 'Target group -> technology -> network'.


1. Serviceprovider as an event in Google Analytics

<script>
 function getIP(json) {
   if (json) {
   	EIPL=json;
   }
   if (EIPL.org) {
   	if (typeof ga === 'function' && typeof ga.getAll() === 'object') {
		ga(ga.getAll()[0].get('name')+'.send', {
		  hitType: 'event',
		  eventCategory: 'eXTReMe-IP-Lookup.com',
		  eventAction: 'ISP',
		  eventLabel: EIPL.org,
		  nonInteraction: true
		});
	}
	else {
		setTimeout(function() {getIP();}, 200);
	}
   }
 }
</script>
<script src="//extreme-ip-lookup.com/json/?callback=getIP" async defer></script>

2. Service provider as Custom Dimension in Google Analytics
Within Google Analytics you must have at least one custom dimension free, because it is used for the service providers. The following steps will help you set that custom dimension:

Within Google Analytics, go to Admin-> property -> custom definitions -> custom dimensions
Click on "+ new custom dimension" and give it a name (for example ISP)
Choose the scope "session"
Click on "create" and on "done"
Remember the number of the dimension, you will need it later (this is the index number, it is behind it)


<script>
 function getIP(json) {
   if (json) {
   	EIPL=json;
   }
   if (EIPL.org) {
   	if (typeof ga === 'function' && typeof ga.getAll() === 'object') {
		ga(ga.getAll()[0].get('name')+'.send', {
		  hitType: 'event',
		  dimension1: EIPL.org,
		  nonInteraction: true
		});
	}
	else {
		setTimeout(function() {getIP();}, 200);
	}
   }
 }
</script>
<script src="//extreme-ip-lookup.com/json/?callback=getIP" async defer></script>


3. Visiting companies directly in Google Analytics

By adding the script below to the website, the companies will return to Google Analytics as an event. So you can see exactly which companies visit the website. There are two basic filters: residentials (ie people who are online at home) and education (educational institutions). With the script below you will only see the companies and educational institutions in Google Analytics.

<script>
function getIP(json) {
   if (json) {
   	EIPL=json;
   }
   if (EIPL.ipType !== 'Residential') {
   	if (typeof ga === 'function' && typeof ga.getAll() === 'object') {
		var org=''+EIPL.org;
                if (EIPL.businessName) {
                	org=''+EIPL.businessName;
			if (EIPL.businessWebsite) {
				org+=' - '+EIPL.businessWebsite;
			}
		}
		ga(ga.getAll()[0].get('name')+'.send', {
		  hitType: 'event',
		  eventCategory: 'eXTReMe-IP-Lookup.com',
		  eventAction: EIPL.ipType, // sends Business, Education as type of visitor
		  eventLabel: org, // sends the Business/Education name and if available the domain of the Business/Education
		  nonInteraction: true
		});
	}
	else {
		setTimeout(function() {getIP();}, 200);
	}
   }
 }
</script>
<script src="//extreme-ip-lookup.com/json/?callback=getIP" async defer></script>
